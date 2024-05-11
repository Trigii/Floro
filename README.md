# Floro: Bypass System Integrity Protecion (SIP) on MacOS With Just Two Commands
This vulnerability allows to bypass **System Integrity Protection (SIP)** and gain **Full Disk Access (FDA)** among other things. It allows to remove the `restricted` flag on any folder on the system with just two lines of code. This vulnerability has no CVE assisnged, and has been tested on macOS 14.0 and earlier. This exploit doesn`t work on MacOS 14.4. Versions 14.2 and 14.3 have not been tested.

If we use `installer` to install `InstallAssistant.pkg` on the system, it extracts the packages on `/Applications/Install macOS Ventura.app`. If before installing the package we create a symbolic link on `/Applications/` called `Install macOS Ventura.app` that points to a folder with the `restricted` flag, `system_installd` removes the `restricted` flag of the folder.

An attacker could remove the `restricted` flag of any folder like `/Library/Application Support/com.apple.TCC/`, move the folder to any other location, and replace the folder with a malicious one containing a custom TCC database.

## Requirements
A terminal with `root` privileges

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

## Impact
This exploit allows an attacker to remove the restrictions on any directory on the file system. Thus, an attacker could move the SIP protected directory to a different location, and replace it with an alternative directory with malicious contents (i.e., replace the TCC database with a malicious one to gain FDA).
