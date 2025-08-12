#### The PID 1 process is the init, which on most newer versions of Linux is just a symbolic link to the systemd program.

#### When a parent process dies or gets killed, and its child process doesn't follow its parent's demise, we call that process an orphan process.

#### Every child process, when terminated, becomes a zombie process and then removed by the parent. When the process exits its existence and releases the resources it had used, its name is still on the OS process table. It is then the parent's process job to remove its name from the process table. When that fails, we have the zombie process, which isn't really a process anymore, but just an entry on the process table of the OS.

#### This is why trying to do a kill command even with the -9 (SIGKILL) option on a defunct (zombie) process doesn't work, because there is nothing to kill

#### So, to kill a zombie process, as in to remove its name from the process list (the process table), you have to kill its parent. For instance, if PID 5678 is a zombie process, and its parent is PID 3452, then to kill the zombie (5678) you end the parent (3452):

```
kill -9 3452
```

#### Finding zombie process

'''
$ ps aux | grep 'Z'
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
tuttu       4586  0.0  0.0      0     0 ?        Z    12:56   0:00 [zypak-sandbox] <defunct>
tuttu       4985  0.0  0.0      0     0 ?        Z    12:57   0:00 [zypak-sandbox] <defunct>


$ ps -o ppid= -p 4586
   4576
tuttu@fedora:~$ ps -o ppid= -p 4985
   4960


$ ps -fp 4576
UID          PID    PPID  C STIME TTY          TIME CMD
tuttu       4576    4575  0 12:56 ?        00:00:07 /app/joplin-desktop/joplin

$ ps -fp 4960
UID          PID    PPID  C STIME TTY          TIME CMD
tuttu       4960    4959  3 12:57 ?        00:07:49 /app/extra/chrome --disable-features=WebAssemblyTrap
'''
