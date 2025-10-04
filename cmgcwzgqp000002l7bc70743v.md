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

<table><tbody><tr><td colspan="1" rowspan="1"><p>Check</p></td><td colspan="1" rowspan="1"><p>What It Means &amp; How to Check</p></td><td colspan="1" rowspan="1"><p>Example (What Success Looks Like)</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Network Reachability</strong></p></td><td colspan="1" rowspan="1"><p><strong>TCP Port 445 (SMB) was open and reachable.</strong> This is the port used for remote administration on Windows. Run this in PowerShell from the scanning machine: <code>Test-NetConnection -ComputerName 192.000.0.000 -Port 445</code></p></td><td colspan="1" rowspan="1"><p><strong>Success:</strong> <code>TcpTestSucceeded : True</code></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Account Status</strong></p></td><td colspan="1" rowspan="1"><p><strong>The local account (</strong><code>admin</code>) was a member of the Administrators group. The scanning account must have full administrative rights on the target host.</p></td><td colspan="1" rowspan="1"><p><strong>Verification:</strong> Open <strong>Computer Management</strong> &gt; <strong>Local Users and Groups</strong> &gt; <strong>Groups</strong> &gt; <strong>Administrators</strong>. Your scanning account must be listed.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Access Policy</strong></p></td><td colspan="1" rowspan="1"><p><strong>The Administrators group was explicitly allowed in the "Access this computer from the network" policy.</strong> This is a Local Security Policy setting required for any remote login.</p></td><td colspan="1" rowspan="1"><p><strong>Verification:</strong> Open <strong>Local Security Policy</strong> &gt; <strong>Local Policies</strong> &gt; <strong>User Rights Assignment</strong> &gt; <strong>Access this computer from the network</strong>. Ensure <strong>Administrators</strong> is listed.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Nessus Credential Format</strong></p></td><td colspan="1" rowspan="1"><p><strong>The simplest username format, </strong><code>admin</code>, was chosen in the policy to avoid naming issues. For workgroup hosts, avoid using <code>DESKTOP-NAME\admin</code>.</p></td><td colspan="1" rowspan="1"><p><strong>Verification:</strong> In your Nessus policy, use the simple username and ensure the necessary services are checked.</p></td></tr></tbody></table>

Even with all of this correct, the "Auth: N/A" error persisted. This told us the issue was a **hidden security policy** blocking the remote logon.

---

## Part 2: The Credential Check Script

Instead of hunting for obscure registry keys, we used a [comprehensive community PowerShell script,](https://github.com/tecnobabble/nessus_win_cred_test/blob/main/credential_check.ps1) `nessus_`[`check.ps`](http://check.ps)`1`, designed specifically to check all low-level Windows settings required for credentialed scanning.

### A. How to Run the Script

1. **Save the Script:** Save the entire script's code (as a `.ps1` file (e.g., `nessus_`[`check.ps`](http://check.ps)`1`) to an easily accessible location on the **Target Windows Machine** (e.g., the `Downloads` folder).
    
2. **Open PowerShell:** Run **PowerShell as Administrator**.
    
3. **Execute:** Navigate to the folder and run the script, passing your scanning account's username:
    
    PowerShell
    
    ```plaintext
    # Navigate to your folder (e.g., Downloads)
    cd C:\Users\LaptopName\Downloads 
    
    # Execute the script
    .\nessus_check.ps1 -ScanningAccounts "admin" 
    ```
    

### B. Analyzing the First Script Run (What We Found)

When we first ran the script, it reported critical warnings that revealed the true blockage:

<table><tbody><tr><td colspan="1" rowspan="1"><p>Script Warning</p></td><td colspan="1" rowspan="1"><p>Problem Identified</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Windows Firewall Configuration</strong></p></td><td colspan="1" rowspan="1"><p>The <strong>WMI</strong> (Windows Management Instrumentation) rules (DCOM-In, WMI-In, ASync-In) were <strong>not enabled</strong> for the active network profile. WMI is how Nessus performs its deep administrative checks.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>LocalAccountTokenFilterPolicy (UAC Only)</strong></p></td><td colspan="1" rowspan="1"><p>Windows User Account Control (UAC) was filtering the remote Administrator's security token, treating the remote login as a standard user. This bypass registry key was <strong>not correctly set</strong>.</p></td></tr></tbody></table>

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