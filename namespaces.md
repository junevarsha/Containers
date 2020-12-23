### Namespaces (unsharing capabilities or controlling what can flow)

```
root@01ec29991918:/my-new-root# ps
    PID TTY          TIME CMD
      1 pts/0    00:00:00 bash
     22 pts/0    00:00:00 ps
```

- Limit visibility,  you can only see your processes

- connected to the same docker container, opens a new shell

```
~|⇒ docker exec -it docker-host bash
root@01ec29991918:/#
root@01ec29991918:/# ps
    PID TTY          TIME CMD
     24 pts/1    00:00:00 bash
     34 pts/1    00:00:00 ps

```

- We want to prevent this:


```
NEW SHELL 

root@01ec29991918:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.1  18508  3464 pts/0    Ss   19:37   0:00 /bin/bash
root          23  0.0  0.0   4568   884 pts/0    S+   19:41   0:00 tail -f secret.txt
root          24  0.0  0.1  18508  3448 pts/1    Ss   19:41   0:00 bash
root          35  0.0  0.1  34404  2788 pts/1    R+   19:42   0:00 ps aux
root@01ec29991918:/# kill 103
bash: kill: (103) - No such process
root@01ec29991918:/# kill 23

```

 - Above kills the long running process in first shell

 ```
root@01ec29991918:/my-new-root# tail -f secret.txt
my super secret thing
Terminated

 ```

 - Install debootstrap

 ```
apt-get update
apt-get install debootstrap -y
debootstrap --variant=minbase bionic /better-root

# head into the new namespace'd, chroot'd environment
unshare --mount --uts --ipc --net --pid --fork --user --map-root-user chroot /better-root bash # this also chroot's for us
mount -t proc none /proc # process namespace
mount -t sysfs none /sys # filesystem
mount -t tmpfs none /tmp # filesystem
```

- This will create a new environment that's isolated on the system with its own PIDs, mounts (like storage and volumes), and network stack. Now we can't see any of the processes!

- child cannot see parent process but parent/host can see/control child process

```
CHILD
root@01ec29991918:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.1  18508  3416 ?        S    19:52   0:00 bash
root          10  0.0  0.1  34400  2900 ?        R+   19:54   0:00 ps aux


root@01ec29991918:/#
root@01ec29991918:/# touch text.txt
root@01ec29991918:/# tail -f text.txt
```

```
PARENT
root@01ec29991918:/# tail -f /my-new-root/secret.txt
my super secret thing
^C
root@01ec29991918:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.1  18508  3472 pts/0    Ss   19:37   0:00 /bin/bash
root          24  0.0  0.1  18508  3452 pts/1    Ss   19:41   0:00 bash
root        7582  0.0  0.0   4520   768 pts/0    S    19:52   0:00 unshare --mount --uts --ipc --net -
root        7583  0.0  0.1  18508  3420 pts/0    S    19:52   0:00 bash
root        7596  0.0  0.0   4568   808 pts/0    S+   19:55   0:00 tail -f text.txt
root        7597  0.0  0.1  34404  2860 pts/1    R+   19:55   0:00 ps aux
```


