---
title: "Fixing Flutter Errors on Windows: When flutter doctor Can’t Find Java, Git, or dartaotruntime.exe"
datePublished: Fri May 02 2025 11:04:18 GMT+0000 (Coordinated Universal Time)
cuid: cma6os8iv001908jj565rczgn
slug: fixing-flutter-errors-on-windows-when-flutter-doctor-cant-find-java-git-or-dartaotruntimeexe
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1746183751639/bab86386-08d7-4ff0-bd3f-0db3e1ecff8e.png
tags: flutter, git, debugging, programming-tips

---

If you’re running into weird issues when setting up Flutter on Windows, like:

* `Unable to find bundled Java version`
    
* `Git not found`
    
* `dartaotruntime.exe missing`
    
* Flutter crashing without warning
    

You’re not alone. I hit all of these back-to-back, and it turned out to be a mix of **missing installations** and **Avira Antivirus silently deleting my files**.

Let me walk you through what happened and how I fixed everything — step by step.

---

### The Very First Problem: Git and Java Weren’t Detected

I was still working on a flutter project when all of a sudden, my git status was no longer showing. So when I ran

```plaintext
git init
```

I got this:

```plaintext
Git not found
```

Then I ran:

```bash
flutter doctor
```

I got this:

```plaintext

[✗] Unable to find bundled Java version
```

Even though I had both installed earlier, something was off.

#### Step 1: Reinstall Git and Java

Just to be sure, I reinstalled both from their official sources:

* **Git**: [https://git-scm.com](https://git-scm.com/)
    
* **Java (JDK)**: [https://www.oracle.com/java/technologies/javase-downloads.html](https://www.oracle.com/java/technologies/javase-downloads.html)
    

Make sure you **install the JDK, not just the JRE**, and that it's a stable version

> Also: After installing, confirm both `git` and `java` are in your system `PATH`. Run `git --version` and `java -version` in your terminal to verify.

---

### Then Came the Flutter Doctor Warnings

After fixing Git and Java, I expected `flutter doctor` to be clean. But nope. I saw:

```plaintext
[!] Android Studio (version 2024.2)
    X Unable to find bundled Java version.
```

It was Android Studio now causing the complaint — not Flutter directly.

#### Step 2: Reinstall Android Studio

Here’s what worked: **Reinstall Android Studio completely.**

This ensures it restores the missing JetBrains Runtime (`jbr`) and Java binaries that Flutter needs.

> In my case, Avira had removed parts of the JetBrains Runtime. More on that below.

---

### Final Blow: Flutter Crashed with Missing `dartaotruntime.exe`

At this point, I thought I was done... until Flutter suddenly refused to run and showed this:

```plaintext
Oops; flutter has exited unexpectedly: 
"ProcessException: Failed to find "C:\dev\flutter\bin\cache\dart-sdk\bin\dartaotruntime.exe"
```

That Dart runtime file was just *gone*. Deleted.

### The Culprit? Avira Antivirus

Avira was silently uninstalling files like:

* `java.exe`
    
* `dartaotruntime.exe`
    
* other `.exe` files in the Flutter toolchain
    

---

### How to Stop Avira from Breaking Your Flutter Setup

#### Step 3: Add Dev Tools to Avira’s Exception List

To stop this from happening again, whitelist your dev tools:

**Steps:**

1. Open **Avira Security**
    
2. Go to **Settings → Security → Protection Options → Real-time protection**
    
3. Add these folders to your excluded folders
    
    ```plaintext
    C:\dev\flutter
    C:\Program Files\Java
    C:\Program Files\Android\Android Studio
    C:\Users\<YourUser>\AppData\Local\Android
    ```
    
4. Also add individual `.exe` files if needed:
    
    ```plaintext
    dartaotruntime.exe
    java.exe
    flutter.bat
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746183517764/a7e9045a-0c07-4a25-89c4-6ca5a8d27b7a.png align="center")
    
      
    

---

### Step 4: Rebuild Flutter Cache (If Files Were Deleted)

If any Dart or Flutter files were removed by Avira:

```bash
flutter clean
flutter pub get
```

Then manually delete:

```plaintext
C:\dev\flutter\bin\cache
```

And let Flutter rebuild everything:

```bash
flutter doctor
```

---

### A quick Summary

| Problem | Solution |
| --- | --- |
| Git not found | Reinstall Git and verify it’s in PATH |
| Java missing in Flutter | Reinstall JDK (Java Development Kit) |
| Java missing in Android Studio | Reinstall Android Studio |
| Flutter crashes – `dartaotruntime.exe` missing | Whitelist tools in Avira and rebuild cache |
| Files silently disappearing | Add dev tools to Avira exceptions list |

---

So, Flutter itself isn’t always the problem , sometimes your **antivirus** is the one making life hard. A few reinstalls plus the right Avira exceptions, and you’re back in business.