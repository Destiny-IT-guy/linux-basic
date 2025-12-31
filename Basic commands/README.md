# basic commands

This will be small guide to use basic linux command.

| command | definition | example |
|---------|------------|----------|
| ls | list directory content | ls `/home/james/` |
| cd | change directory | cd `/etc/` |
| touch | create file | touch file.txt |
| mkdir | create folder | mkdir `/home/james/FTP` |
| rm | delete folder | rm `/home/james/FTP` |
| cp | copy file | cp `home/james/file.txt` `/home/james/FTP/.` |
| mv | move file | mv `home/james/file.txt` `/home/james/FTP/.` |
| scp | copy file over ssh | scp `file.txt` `root@IP:/home/paul/.` |
| pwd | print name of current directory | pwd the output `/home/test` |
| whoami | shows current user | whoami |
| id | shows uid gid and groups | id or id root |
| cat | print file lines | cat script.sh |
| tree | show directory in tree-like format | tree `/home/test` |
| man | reference manual for commands | man mkdir, man -f scp |
| stat | display file status | stat test.txt |
| file | determine file type | file test.yaml | 
| du | directory size in kilobyte by default | du or du `/home/test` or du -h |
| df | display used and free disk space on system folders | df -h or df -h `/run` |
| history | show command history | history |
| clear | clear terminal screen | clear |
| sudo | run command as root user | sudo apt install vim |
| echo | print words on screen | echo hello |


## files command
Here I will show you commands to view and manipulate files.

The <b><u>less</u></b> command will allow you to view a file page by page. This is very useful when reading log files with thousands of lines. Here is a basic way to navigate while using the command.

| key | action |
|-----|--------|
| space | Goes to next page |
| b | Goes to previous page |
| g | Goes to start of file |
| G | Goes to end of file |
| ↑↓ | Goes up and down one line at a time |
| ←→ | Goes left and right |
| q | exit the file |

The <b><u> head</u></b> command will output the first part of a file. There are a couple of option.

| command option | action |
|-----|--------|
| head | The command will show the first 10 lines of a file |
| -n number | The n option will show the first x amount of lines you chose |
| -c number | The c option will show the first x amount of bytes in your file |
| head file1 file2 | The head command can be run with multiple files and will show a header |
| -q | The q option will remove header when running head with multiple files |
| -v  | The  `-v` option will always show header even if we are using one single file |

The <b><u> tail</u></b> command will output the last part of a file. This is the exact opposite of the head command and the option are mostly the same so I won't explain them again but there is a useful feature unique to tail. The `-f` option allows you to follow last line of a file in real time. This is an example of how to use it

```bash
tail -n 20 -f syslog.log
```

This will show the last 20 lines of the syslog file in real time.

The <b><u> find</u></b> command will search for file in a directory. This command has many option but I will just show the basic. This is how you use this command.

```bash
find [path] [option] [expression]
```

I will show you of a table of basic expressions and option you can use.

| option/expression | definition |
|---------------------|------------|
| -name "test.txt | this will search for the file test.txt in the path you selected. The name will be case sensitive so Test.txt wont appear |
| -iname "test.txt | This is like name but isn't case sensitive so TEST.txt will work |
| -type (d or f) -name ssh | this will search for file or name that follow the name requirement. The d option is for folder and f is for files |
| -type f -name "*.yaml" | This will look for files that has the extensions you chose. This will show all yaml files.|
| -type f -mtime -7 | this will show files that were modified within the last 7 days. +7 will show files that were modified more than 7 days ago. |
| -type -d -empty | This will show folders that are empty, no files inside them. You can also use -f for empty files |
|-maxdepth 1 -type f | will only search for files in the current folder |
| -type f -size +10M | this will show files that are larger than 10 megabyte. You can use -10M for under or 10M for exactly 10 megabyte. K= kilobyte G= gigabyte c = byte|
| -type f -user root | This will show files that the owner is the root user. You can also use -group option | 
| -type d -perm 775 | this will show folder that has 775 permissions |

There is one more find option that is very powerful. The option is the `-exec` option. This is a simple breakdown of the exec command.

```bash
find -path [option] [expression] -exec cp {} /home/backups/ \;
```
The beginning of the command stays normal you use other options and expression before reaching the exec option. The -exec makes the following command on each file that match the description. The {} is a placeholder for the file name. The \; tells the system to end the command.

The <b><u>grep</u></b> command will print lines in a file that match a pattern you define. Here is option on how to use grep

| option | definition |
|--------|------------|
| grep "hello" test.txt | This will print lines in text.txt that have the word hello. This is case sensitive |
| -i "hello" ... | This will print lines that include hello but isn't case sensitive |
| -w "is" ... | This will show whole word; will not match words that contain `is` as part of them |
| -n "hello" ... | This will also show line numbers |
| -r "hello" `/path` | This will search in all files in the folder and its subfolders. The -rl option will only list filenames |
| -v "hello" ...| This will show lines that don't have the word hello |
| -c " hello" | This will count how many lines has the pattern |

You can also use the grep method with pipe `|` to filter another command output.example 

```bash
df -h | grep "/dev/*"
```

There are other command that allow you to manipulate files like sed, cut, tee. I might explain them later in another folder

## System and process

These are a couple of basic system command you might use regularly

| command | definition | example |
|---------|------------|---------|
| ps | running processes | ps aux |
| top | show live process | top |
| htop | needs to installed but better version of top | htop |
| uptime | show how long system has been running | uptime |
| free | show amount of used and free memory | free -h |
| kill | kill/ terminate process | kill 1234 or kill -9 1234 |

## networking commands

These are a couple networking command. Some commands need to be installed manually

| command | definition | example |
|---------|------------|---------|
| ip a | shows IP address information | ip a |
| ping | test connectivity | ping google.com |
| traceroute | print the route packets destination before reaching the destination | traceroute 8.8.8.8 |
| ss | show port status | ss -tulpn |
| hostname | system hostname | hostname |
| ip route | show routing table | ip route |
| resolvectl | resolve dns name | resolvectl status |
| mtr | ping and traceroute | mtr google.com |
| arp | display and manipulate arp table | arp -a |
| nmap | scan network for open ports OS and more | nmap 192.168.0.1 |
| ethtool | display or manipulate ethernet device | ethtool eth0 |

There are other networking told you can use like DNS tools like ( nslookup and dig) or packet capturing tools like (tcpdump or wireshark).

## Package management Debian

This is common package management tools use for debian base distro

| command | definition | example |
|---------|------------|---------|
| apt install | install app | apt install vim |
| apt update | refresh packages | apt update |
| apt upgrade | upgrade packages | apt upgrade -y |
| apt remove | remove packages | apt remove htop |
| dpkg | Debian package manager; the following command is to list all packages | dpkg -l |

I didn't show every command option. To view every command option, use the man manual or you can use the command following with `--help`.