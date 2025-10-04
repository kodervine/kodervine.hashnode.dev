---
title: "Nessus Auth N/A Fix: Windows Workgroup Credentialed Scan"
datePublished: Sat Oct 04 2025 23:38:43 GMT+0000 (Coordinated Universal Time)
cuid: cmgcwzgqp000002l7bc70743v
slug: nessus-auth-na-fix-windows-workgroup-credentialed-scan
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1759620416400/b0fa77f6-6f29-482b-bb9f-67501e98cdc5.png
tags: windows, vulnerability, troubleshooting, cybersecurity-1, vulnerability-management, nessus

---

If you're reading this, you've probably hit a frustrating wall in vulnerability scanning: **Nessus is showing "Auth: N/A" (Not Authenticated)** on your Windows target, even though you're sure the username and password are correct.

This is a notorious problem when scanning a standalone Windows machine (a **Workgroup** member, not joined to a Domain). Windows has built-in security features designed to block remote local administrators, which breaks Nessus's ability to run its deep, credentialed checks.

We're going to break down the entire fix, including the use of a powerful script that was the final key to our success.

---

## Part 1: The Problem and Initial Setup

The goal of a credentialed scan is simple: log in remotely to your target machine (e.g., `192.000.0.00`) to check patch levels, registry settings, and installed software. If authentication fails, Nessus can only perform a high-level network port scan, which is almost useless for compliance and patch management.

### Initial Checks We Performed:

Before we get into the fixes, we confirmed the basics were already in place:

| Check | What It Means & How to Check | Example (What Success Looks Like) |
| --- | --- | --- |
| **Network Reachability** | **TCP Port 445 (SMB) was open and reachable.** This is the port used for remote administration on Windows. Run this in PowerShell from the scanning machine: `Test-NetConnection -ComputerName 192.000.0.000 -Port 445` | **Success:** `TcpTestSucceeded : True` |
| **Account Status** | **The local account (**`admin`) was a member of the Administrators group. The scanning account must have full administrative rights on the target host. | **Verification:** Open **Computer Management** &gt; **Local Users and Groups** &gt; **Groups** &gt; **Administrators**. Your scanning account must be listed. |
| **Access Policy** | **The Administrators group was explicitly allowed in the "Access this computer from the network" policy.** This is a Local Security Policy setting required for any remote login. | **Verification:** Open **Local Security Policy** &gt; **Local Policies** &gt; **User Rights Assignment** &gt; **Access this computer from the network**. Ensure **Administrators** is listed. |
| **Nessus Credential Format** | **The simplest username format,** `admin`, was chosen in the policy to avoid naming issues. For workgroup hosts, avoid using `DESKTOP-NAME\admin`. | **Verification:** In your Nessus policy, use the simple username and ensure the necessary services are checked. |

Even with all of this correct, the "Auth: N/A" error persisted. This told us the issue was a **hidden security policy** blocking the remote logon.

---

## Part 2: The Credential Check Script

Instead of hunting for obscure registry keys, we used a [comprehensive community PowerShell script,](https://github.com/tecnobabble/nessus_win_cred_test/blob/main/credential_check.ps1) `nessus_`[`check.ps`](http://check.ps)`1`, designed specifically to check all low-level Windows settings required for credentialed scanning.

### A. How to Run the Script

When running a PowerShell script (`.ps1` file) manually, the default security setting (Execution Policy) often prevents it from running. You need to temporarily bypass this policy for the duration of the command.

1. **Save the Script:** Save the entire script's code (e.g., as `nessus_`[`check.ps`](http://check.ps)`1`) to an easily accessible location on the **Target Windows Machine** (e.g., the Downloads folder).
    
2. **Open PowerShell:** Run PowerShell as **Administrator**.
    
3. **Enable Execution:** Run the `-ExecutionPolicy Bypass` flag. This flag is applied only to this specific command instance (`-Scope Process` is implied when using this flag on the `powershell.exe` executable), ensuring the policy reverts immediately afterward.
    
    ```plaintext
    # Execute the script with a temporary policy bypass
    Set-ExecutionPolicy Bypass -Scope Process -Force
    ```
    
    *Replace* `C:\Users\LaptopName\Downloads` *with the actual path to your saved script and* `"admin"` *with the correct scanning account username.* Running the above script ensure you are launching the PowerShell engine (`powershell.exe`) and telling it, "For this one file execution only, ignore the security policy (`-ExecutionPolicy Bypass`), then run the file (`-File ...`)." This is the most common and non-permanent way to run an administrative script locally without changing the system's security settings.
    
4. **Execute:** Navigate to the folder and run the script, passing your scanning account's username:
    
    ```plaintext
    # Navigate to your folder (e.g., Downloads)
    cd C:\Users\LaptopName\Downloads 
    
    # Execute the script
    .\nessus_check.ps1 -ScanningAccounts "admin"
    ```
    

*Replace* `C:\Users\LaptopName\Downloads` *with the actual path to your saved script and* `"admin"` *with the correct scanning account username.*

### B. Analyzing the First Script Run (What We Found)

When we first ran the script, it reported critical warnings that revealed the true blockage:

| Script Warning | Problem Identified |
| --- | --- |
| **Windows Firewall Configuration** | The **WMI** (Windows Management Instrumentation) rules (DCOM-In, WMI-In, ASync-In) were **not enabled** for the active network profile. WMI is how Nessus performs its deep administrative checks. |
| **LocalAccountTokenFilterPolicy (UAC Only)** | Windows User Account Control (UAC) was filtering the remote Administrator's security token, treating the remote login as a standard user. This bypass registry key was **not correctly set**. |

---

## Part 3: Applying the Final Fixes

Based on the script's findings, we applied two final, essential fixes to the **Target Windows Machine**

### 1\. Enable WMI Firewall Rules

Nessus performs deep checks using Windows Management Instrumentation (WMI), which requires specific inbound firewall rules. The script found these were missing.

**Action Steps:**

1. Open the **Windows Defender Firewall with Advanced Security**: Press **Win + R** and type `wf.msc`.
    
2. In the left-hand pane, click on **Inbound Rules**.
    
3. Filter or scroll to find the built-in rules for **Windows Management Instrumentation**.
    
4. Right-click and **Enable Rule** for the three rules related to WMI access. Ensure the **Enabled** column is set to **Yes**:
    
    * **Windows Management Instrumentation (DCOM-In)**
        
    * **Windows Management Instrumentation (WMI-In)**
        
    * **Windows Management Instrumentation (ASync-In)**
        

### 2\. Set the UAC Remote Bypass Key

This is done via the command line to ensure precision:

```plaintext
# This command adds the registry value '1' to bypass UAC token filtering for remote local accounts.
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1 /f
```

### 3\. Verification Run

After applying the fixes, we ran the `nessus_`[`check.ps`](http://check.ps)`1` script again. This time, **it reported "No changes needed. Correct configuration" for ALL checks**. This confirmed the target host was finally ready.

```plaintext
Report for DESKTOP, part of WORKGROUP

--------------------------------------------------------
Checking "Enable Remote File Shares - Server"

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "Enable Remote File Shares - Client"

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "Ensure 'Network access: Sharing and security model for local accounts' is set to 'Classic - local
        users authenticate as themselves'"

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "'Windows Management Instrumentation' Service must be set to Automatic"

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "'Server' Service must be set to Automatic"

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "'Remote Registry' Service should be set to Automatic or Manual"

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "Ensure the proper user/group is in the local Administrator user group."

admin is configured correctly.

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "Windows 10 > 1709 - Server SPN Validation Enabled"

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "Is Symantec Endpoint Protection Installed?"

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "Windows Firewall Configuration"

No changes needed. Correct configuration.
--------------------------------------------------------

--------------------------------------------------------
Checking "LocalAccountTokenFilterPolicy (UAC Only)"

No changes needed. Correct configuration.
--------------------------------------------------------
```

---

## Part 4: The Final Scan

The final step was a moment of truth. We made sure the Nessus policy was configured to leverage the fixes:

* **Username:** Simple `admin`
    
* **Plugins:** Enabled **"Start the Remote Registry service during the scan"** and **"Enable administrative shares during the scan"**.
    

The final Nessus scan immediately resulted in **Auth: Pass** for the target machine

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759619914457/8dafd48a-e242-4221-b586-93003541a59b.png align="center")

### Conclusion: Continuous Scanning

To keep this working for all future scans, **leave all the fixes permanently applied** on the target machine. Disabling the UAC bypass or the WMI firewall rules will cause the frustrating "Auth: N/A" error to immediately return.