### Change Root (chroot - unsharing filesystem)

- Problem: one company should not steel secret recipie from other company

- Solution: Change roots or Linux Jail (jailing a process to particular part of the OS)

- Starts a ubuntu VM and drops me inside
```
docker run -it --name docker-host --rm --privileged ubuntu:bionic
```

- version and user system
```
root@79f5c55d6c6c:/# cat /etc/issue
Ubuntu 18.04.5 LTS \n \l

root@79f5c55d6c6c:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
```

- Goal: process limited to one folder in the OS
```
mkdir /my-new-root

echo "my super secret thing" >> /my-new-root/secret.txt

root@79f5c55d6c6c:/# chroot /my-new-root bash
chroot: failed to run command 'bash': No such file or directory

```
- That's because bash is a program and your new root wouldn't have bash to run (because it can't reach outside of its new root.) Lets fix it

```
root@79f5c55d6c6c:/# mkdir /my-new-root/bin

root@79f5c55d6c6c:/# cp /bin/bash /bin/ls /my-new-root/bin/

root@79f5c55d6c6c:/# chroot /my-new-root bash
chroot: failed to run command 'bash': No such file or directory

```
 - Oops, these depend on some libraries, lets add those

```
root@79f5c55d6c6c:/# mkdir my-new-root/lib{,64}

root@79f5c55d6c6c:/# ldd /bin/bash
	linux-vdso.so.1 (0x00007ffdbe505000)
	libtinfo.so.5 => /lib/x86_64-linux-gnu/libtinfo.so.5 (0x00007fb22fd8b000)
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007fb22fb87000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fb22f796000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fb2302cf000)

root@79f5c55d6c6c:/# cp /lib/x86_64-linux-gnu/libtinfo.so.5 /lib/x86_64-linux-gnu/libdl.so.2 /lib/x86_64-linux-gnu/libc.so.6 /my-new-root/lib

root@79f5c55d6c6c:/# cp /lib64/ld-linux-x86-64.so.2 /my-new-root/lib64

```

 - Repeated the above steps for ls and cat

```
chroot /my-new-root bash 

ls

cat

```

- You should successfully see everything in the directory. Now try pwd to see your working directory. You should see /. You can't get out of here! This, before being called containers, was called a jail for this reason. At any time, hit CTRL+D or run exit to get out of your chrooted environment.