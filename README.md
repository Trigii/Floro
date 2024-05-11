# Floro
This repository contains a 0day vulnerability for MacOS, discovered on 05/11/2024. This vulnerability allows to delete the `restricted` attribute of any folder on the system, allowing to bypass **System Integrity Protection (SIP)**.

---
## Requirements
In order to execute the exploit, `root` privileges are required.

---
## Steps

1. [Download](https://mrmacintosh.com/macos-ventura-13-full-installer-database-download-directly-from-apple/) InstallAssistant.pkg
2. Modify the variable `TARGET_DIR` to a SIP protected directory (default target is the system TCC database directory).
3. Give the exploit execution permissions:
```sh
$ chmod +x exploit.sh
```
4. Run the exploit:
```sh
$ ./exploit.sh PATH_TO_INSTALLED_PKG
```
5. You should now be able to see the that the `TARGET_DIR` dont have the `restricted` flag with the following command:
```sh
$ ls -ldO TARGET_DIR
```
---
## Impact
This exploit allows an attacker to remove the restrictions on any directory on the file system. Thus, an attacker could move the SIP protected directory to a different location, and replace it with an alternative directory with malicious contents (i.e., replace the TCC database with a malicious one to gain FDA).