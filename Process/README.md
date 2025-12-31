# Process

A process is a running instance of a program. They are created when you run a command or open an application. The system manages processes by allocating resources depending on the process priority.

Each process is unique and can be identified by their Process ID (PID). This is a unique number assigned by the system. Every process. A process also has a parent process (PPID) which indicate which process created it. Also every process is owned by a user. This ownership is used for security reasons and determining what the process is allowed to accomplish.<br> On most systems the first process is  init or systemd which has a PID of 1. Its PPID is 0 which is the Linux kernel.

### Process state

Every process has a state which indicate what is this currently doing. You can see process state while looking at the stat column while using the commands like `ps aux`, `top` or `htop`.

| Code | state | Definition |
|------|-------|------------|
| R | Running| The process is currently running using resources or is ready in the queue |
| S | sleep | the process is waiting for resources. The process can be interrupted |
| D | Uninterruptible Sleep | Process cannot be interrupted and is waiting for I/O operations to complete. |
| T | Stopped | The process was stopped usually by someone pressing `ctrl+Z`. You can continue the job by pressing `fg` |
| Z | Zombie | The process is complete but parent hasn't read his exit status.
| I | idle | idle kernel thread | 

While using the `ps` command you might see additional modifiers in the stat column. These letter and special characters provider additional information about the process. Here is a little presentation of these modifiers.

| Modifier | definition |
|----------|------------|
| `<` | High-priority (not nice to other users). |
| `N` | Low-priority (nice to other users). |
| `L` | Has pages locked in memory. |
| `s` | Is a session leader |
| `l` | Is a multi-thread process. |
| `+` | Is in the foreground process group. |

## Process roles

Processes can be separated into two groups. First there is <u><b>foreground process</b></u>. They are interactive tasks that take over your terminal so you must wait until they are finish before you can enter other commands. <b><u>Background process</u></b> run independently of the terminal. No user interaction is needed. You can run a process in the background by adding `&` at the end of your command.

Process can also be categorize by their roles and relationship. Here ia a list of main processes.

| Process type | definition |
|--------------|------------|
| Parent process | A process that creates other projects (child process) |
| Child process | Process created by parent process. It inherits attributes from parent |
| Orphan process | child process whose parent has been terminated. The orphan process is still running |
| Zombie process | completed process that is waiting for parent hasn't acknowledge his exit status. |
| Daemon process | Background process with no user interaction. For system services. |

### Process Lifecycle

Linux process goes through stages between its creation and termination. Here is a brief visual representation

| Name | Definition |
|------|------------|
| Creation | process is created when user runs a command. |
| Ready | Process is waiting for CPU allocation |
| Running | Process is executing on the cpu |
| sleeping | waiting for an event |
| stopped | pause usually be user interaction |
| Termination | process completed task or is killed. releases resources.

## ressources

Every process require resources allocation to execute. These resources are CPU time, memory usage, disk space , network access and more. Linux kernel manages theses resources to ensure stability across the system. Each process is assigned a priority (nice value). The lower the value is the higher the priority it has. The 2 extreme are -20 and 19 where -20 is the highest priority.

In some tools like top you might see a PR value. This stands for priority and determine the scheduling priority of the process. This value is influence by the nice value and other factors.

## Interacting with process

In linux you can interact with process using signals, IPC and other commands. These tools let you monitor, stop, resume and terminate processes as needed.

### basic command

You can monitor system process with the `ps` command. This command shows a snapshot of running processes. If you want to see all process with additional information you can use the following option.

```bash
ps aux
```

The `top` command display real type process and continuously updating process information.

`htop` is a interactive version of top. With additional features.

`btop` is a more advance version of htop with detail cpu and memory information.

i haven't tried `atop` yet but it seems to also monitors logs with processes.

### Job control

When you are running a command you can move it to the background or foreground with these following commands.


| Command | Explanation |
|---------|-------------|
| command `&` | Runs command (process) in background |
| `jobs` | Show active jobs in the current terminal |
| `fg %<job_number>` | Resume jobs in foreground |
| `bg%<job_number>` | Resume jobs in background |
| `ctrl+z` | Pause process |
| `ctrl+c` | Terminates process |

here is an example i wrote 2 commands and stop them with `ctrl+z`. Here is what I see.

```bash
:~$ jobs
[1]-  Stopped                 sudo apt update
[2]+  Stopped                 sudo apt install ploop -y
```

If I want to start job 1 in background I will use the following command. `bg %1`. Job numbers are sticky. They do not change if a job with a lower number is completed. So if I wanted to resume job 2 in foreground I would do `fg %2`.

### Termination process

To terminate process you can use variation of the kill command. Here they are.

| Command | Explanation |
|---------|-------------|
| `kill <PID>` | kill process |
| `kill -9 <PID>` | SIGKILL to force terminate process |
| `pkill <process name>` | kill process by names |
| `killall <process name>` | kill all process with the name you entered. |

### Other commands

There some other command you might use will dealing with process. The `nice` command can start a process with a specific command. Here is an example.

```bash
nice -n 10 command
nice -n 10 sudo apt update
```

Also negative nice number only work if you are using root user or with sudo.

To change a current process nice value we can use the renice command.

```bash
sudo renice -n 5 -p <PID>
jobs -l
sudo renice -n 5 -p 3913
```

Then you can verify with htop easily

also `grep` let you search process by name.