# Files and Folders

In linux everything is a file even folders and end device like external hard drives, keyboards  etc. There are many type of file but I will separate them into 7 different groups

## file type

| symbol | file type   | what is it | Usual location |
|--------|-------------|------------|----------------|
|  - |  Regular file| text, images,script,| anywhere on system|
| d | Directory | Folder | anywhere on system |
| l | Symbolic link | shortcut to other file | `/bin,` `/lib,` `/usr/bin` |
| b | Block files   | hard drive , usb  | `/dev` |
| c | Character device | keyboard, monitors, etc | `/dev` |
| s | Socket files | inter-process communications (IPC) | `/var/run`, `/run` |
| p | named piped file | process communication | `/tmp`, `/run` |

To be able to see file symbol use the `ls -l` command in terminal and you should see something like this.

```bash
drwxrwxr-x 2 test test      4096 Dec  7 23:47 FTP
-rw-r--r-- 1 root root        17 Dec  8 15:00 localfile.txt
```

### file ownership

Each file has 3 types of ownership.

| Type    | definition  | example |
|---------|-------------|---------|
| Owner   | The user who own the file.| root |
| Group   | A group of users associated with the files | admins |
| Other   | Every other user who isn't the owner or in the group | anyone |

Depending on file ownership, the user will have different actions he can perform on the files. There are 3 types of permission. These are

| Permission | for files | for directories | number |
|-----------|-----------|-----------------|--------|
| r(read) | can read file | can list folder content | 4 |
| w(write) | can modify or delete file | can create and delete folders | 2 |
| x(execute) | can run file as script | can enter inside folder | 1 |

so let me explain the following file ownership

```bash
drwxrwxr-x 2 test test      4096 Dec  7 23:47 FTP
```

The files named ftp is a folder. The first set of letter define the permission of the file owner. The second define permission of the group and third define other user. The first test define the owner of the file the second define the Group. If I wanted to manually do all of that to a new directory I would do the following.

```bash
chmod 775 FTP/
chown test:test FTP/
```

## Folder

Linux Folder system is like a tree. Everything is under the root directory.

- `/`
  - `/home`
  - `/etc`
  - `/lib`
  - `/mnt`

here is a basic description for folders that are directly under the `/` folder 

| Folder | Description |
|--------|-------------|
| `/home` |  Personal folder for each User |
| `/etc` | system and apps configuration files |
| `/bin` | Essential binary executable | 
| `/opt` | Optional or 3rd party package software |
| `/tmp` | Temporary location (cleared on reboot) |
| `/var` | Folder that contains Log files |
| `/boot` | Files needed for the boot process |
| `/dev` | Special or device files |
| `/lib` | Hold shared libraries necessary to boot the system |
| `/media` | Mount points for removable media such as DVDs, USB sticks, etc |
| `/mnt` | Mount point for temporary filesystems |
| `/run` | Information of system since it was booted |
| `/sbin` | Holds commands needed to boot the system but not executable by regular users |
| `/srv` | It contains server-specific and server-related files |
| `/sys` | Provides information about kernel and structured |

To see a more in dept description of the following you can check the hierarchy with the command `man hier`.