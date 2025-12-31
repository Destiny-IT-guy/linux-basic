# Users

 A user is an individual account that can log in to the system. Each linux user has a username, a user id (uid), a home directory, and a default shell. You can assign users to group which help manage permission access to files and folders. Before I continue I must explain all users type. There are 3 type of users in linux.

 | type | definition |
 |------|------------|
 | root | root is the superuser. He has full control of the system and has a uid of 0. |
 | Regular users | non admin users created for every day work. Usually has limited permission, |
 | System users | User created by the system used for running services like web servers, database etc. |

## User creation and modification

### user creation

There are 2 command we can use to create a user. They are the following

```bash
sudo adduser username
sudo useradd username
```

It is recommended to use adduser because it is interactive. It forces you to enter a password for the user and it will create a home directory for the user. useradd doesn't prompt you to create a password. Here is another better way to use useradd

```bash
sudo useradd -m -s /bin/bash username
sudo passwd username
```

The `-m` will create a home directory and the `-s /bin/bash` will set bin bash at the user shell. Finally passwd will prompt you to set the user password.

### modify user

To change password you can also use `passwd` command.

<b><u>usermod</b></u> is a tool used to modify user account. There are many option for this command so I will make a table to list off some basic option

basic syntax<br>
`sudo usermod [option] username`

| option | definition | example |
|-----|---------|-------|
| -l newname | change username | `sudo usermod -l james ubuntu` This will change username from ubuntu to james |
| -d /new/home/ -m | change home directory and move files | `sudo usermod -d /test/james -m james` This will move all files of james current home directory to /test/james |
| -s /bin/bash | change login shell | `sudo usermod -s /usr/sbin/nologin james` change shell of james |
| -a -G group | add user to group without removing from other groups | `sudo usermod -a -G FTP james` James was added to the group named FTP |
| -L | lock user account ( disable login) | `sudo usermod -L james` lock james account |
| -U | Unlock user account | `sudo usermod -U james` unlock james account |
| -u  UID | change user UID | `sudo usermod -u 1700 james` change james UID to 1700  |

### delete user

To delete user use one of the following command:

```bash
sudo userdel username
sudo deluser username
```

you can read command manuel to see option.

### Groups

Here are basic groups command.

| option | definition | example |
|-----|---------|-------|
| groupadd| Create new group | `sudo groupadd FTP` |
| groupdel | delete a group | `sudo groupdel  FTP` |
| getent group group| list all user of a specific group | `getent group FTP` |

## sudoers

The sudoers file is a configuration file located at /etc/sudoers that controls privileges for users and groups to execute command as other user. This is often use to gain root privileges.<br><br> 
To avoid syntax errors that can cause great problem, always use `visudo` command to edit sudoers file. You could change `/etc/sudoers` files permission and modify it from there but that isn't recommended

### basic sudoers syntax

This is a basic user syntax for sudoers

```bash
USER HOST=(RUN_AS) COMMANDS
```

Real example

```bash
james ALL=(ALL) ALL
```

Explanation. The user is james. The first ALL determines which hosts the rule applies to.  
The second ALL determines which user(s) the command can run as.  
The third ALL determines which commands can be run.

 Let say I want to be more specific I want my user be able to only run this command systemctl restart apache2 as root user from a specific ip I would use this.

 ```bash
 james 192.168.2.24=(root) /usr/bin/systemctl restart apache2
 ```

 If I wanted to allow to restart any services I could use * after the restart like this.

```bash
....systemctl restart *
```

Not all commands are in /usr/bin so make sure to find the correct path to the commands. You can use the `which` or `type` command to figure out path to a specific command 

This would allow him to restart other services installed on the server like mysql and others.

### sudoers.d

Instead of putting all sudoers configuration in a single file you can create extra file in /sudoers.d to better manage your sudoers configuration. All files in this folder act as if they were sudoers file

## important /etc files

There 3 to 4 import /etc files that are useful for user and group info.

### /etc/passwd

This is a file that stores basic information about users. Each line represent a user. No password are stored here. This is an example of 2 line in this file

```bash
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

each line has 7 different fields separated by `:`.

| column number | column name | explanation | example |
|---------------|-------------|-------------|---------|
| 1 | username | User's login name | root or daemon |
| 2 | password | placeholder for password. It will be x if password in /etc/shadow | x |
| 3 | UID | User id unique number for each user. Regular user UID will be ≥ 1000 | 0 or 1 or 1000 |
| 4 | GID | Primary group id | o or 1 or 1000 |
| 5 | GECOS | optional info like full name, phone and more. Often blank | ::  | 
| 6 | Home directory | Path to user home folder | /root or /home/user |
| 7 | shell | The shell executed upon login | /bin/bash |

a easy way to change GECOS field use this command `sudo chfn username`

####  shell type

common shell type

| shell | Description |
|-------|-------------|
| `/bin/bash` | Standard. Default on most systems. Good for daily use and scripting |
| `/bin/zsh` | z shell. Has some advanced feature. Auto completion |
| `/bin/dash` | a faster version of `/bin.sh` |

No-login shells are used to prevent user to connecting to those account but system can still use account for services and own files. Here are some you might see.

| shell | Description |
|-------|-------------|
| `/usr/sbin/nologin` | Denies login to user but allow account to be used for services. Show a message when you try to login |
| `/sbin/nologin` | same as above just a shortcut |
| `/bin/false` | Prevent login silently. Doesn't show a message when you try to login |

Here is another way to change shell type :

```bash
chsh -s /bin/bash
```

## etc/shadow

This file stores encrypted password and password policies. Like /etc/passwd each line represent a single user.

here is an example of a line in the file :

```bash
root:*:20135:0:99999:7:::
daemon:*:20135:0:99999:7:::
```

| column number | column name | explanation | example |
|---------------|-------------|-------------|---------|
| 1 | username | login name | root or james |
| 2 | password | hashed password or symbols | ! or * or $6$..... |
| 3 | last change | days since last password change. reference day is jan 1st 1970 | 20444 |
| 4 | Minimum | Minimum number of days required before password user can change his own password | 0 or 90 |
| 5 | Maximum | Maximum number of days password is valid. User will be force to change password when its at 0 | 0 or 90 or 99999 |
| 6 | Warn | Number of days that user is warned before password expire and must b changed | 7 |
| 7 | Inactive | number of days after password expired before account is disabled | 31 |
| 8 | expired | The exact date the account was expired. Reference date is also jan 1st, 1970 | 20420 |

the 2nd field is the hash password field. The field starts with an ID after the first dollar signs. This value determine which hash algorithm what use. Here is a basic of the main values

| id | hash algorithm |
|----|----------------|
| $1$ | MD5 |
| $2a$ | blowfish |
| $5$ | SHA-256 |
| $6$ | SHA-512 |
| $y$ | yescrypt |

To modify future hash for future password use the following method

Go in the file `/etc/login.defs`

search for `ENCRYPT_METHOD`.

Update it to one of the following

```bash
ENCRYPT_METHOD SHA512
ENCRYPT_METHOD YESCRYPT
```

Then save and exit.

To manually generate the hash you can use the following method for SHA-512

```bash
openssl passwd -6
```

It will ask you to enter your password twice then show the hash version 

To generate yescrypt hash do the following: 

```bash
apt install whois
mkpasswd -m yescrypt
```

### password symbols

There are 3 symbols you might see. This is a brief explanation.

| symbol | Meaning | Description |
|---|----|----|
| !! | Password never set | Account was created but password wasn't set |
| * | Locked/System | System account. Can't be used for login |
| ! | Locked | The account was most likely locked with usermod -L |

### change value

Let say you want to change the min, max, warning or inactive value for a user. You can use the `chage` command here is a quick guide

| field | option | example |
|-------|--------|---------|
| Min | -m | `sudo chage -m number username` |
|  Max | -M | `sudo chage -M number username` |
| Warning | -W | `sudo chage -W number username` |
| Inactive | -I | `sudo chage -I number username` |

## /etc/group

stores group information. Like the other one line per group.

here is a brief example:

```bash
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog
```

| column number | column name | explanation | example |
|---------------|-------------|-------------|---------|
| 1 | Group name | Name of group | root,sys |
| 2 | password | placeholder for password stored in `/etc/gshadow` | x |
| 3 | GID | Group ID | 1, 4, 1000 |
| 4 | user list | name of users in the group separated by commas | root, james |

## Change user

In linux you can use the `su command to change user. Like this:

```bash
su james
```

you can also use this option to make sure you switch to the user environment.

```bash
su - james
```

If you want to connect to the root user use the following command:

```bash
sudo su -
```

lastly if you want to go back to previous user just type `exit`.