# Introduction

## Why containers are a relevant topic for any software engineer?

Honestly, there's no single concept of a "container": it's just using a 3 diff. kernal features of Linux together culminate to form Containers.

### Why do we need Containers?

Bare metal: 
   - you control your destiny - not scalable, expensive - capacity - more secure - DELL, EMC, Hardware Providers - few advantages, lot of downsides.

VM: 
   - 1 server - multiple different OS running at the same time as virtual servers - Manage capacity - virtualization strategy - scale up and down at our capacity 
   - Massive Security problems if two VMS in a server are not seperate from each other. example: coca-cola and pepsi shared recipie - fork bomb
   - performance problems - overhead - ten server - ten copies of OS - wasting capacity on OS - hinting at containers here

Public Cloud: 
   - GCP, AWS, Azure. Hard part is they're still just giving you machines - you have to manage all the software networking, provisioning, updating etc
   - Cheaper as there is no need to pay to people to manage physical data center

Containers give us many of the security and resource-management features of VMs 
   - "Light weight"
   - freezing file system and dumping it to a container image - host OS executes these containers
   - Without the cost of having to run a whole other OS.
   - It instead uses chroot, namespace, and cgroup to separate a group of processes from each other. 
    
