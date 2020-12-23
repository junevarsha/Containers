### Why do we need containers?
---
- Honestly, there is no single concept of "container"
- It is just using a 3 different kernal features of Linux together culminate to form Containers.
        * chroot
        * namespaces
        * cgroups

### Journey of Containers
---
- Bare metal(hardware providers): you control your destiny
    * Scalablility is not instant
    (Call up Dell or IBM and ask them to ship you another one, then get your tech to go install the phyiscal server, set up the server, and bring into the server farm. That only takes a month or two right? Pretty much instant! )
    * Expensive (people cost)
    * This is great if you're extremely performance sensitive and you have ample and competent staffing to take care of these servers

- Virtual Machines: 
    * 1 server, multiple different OS running at the same time as virtual servers
    * Manage capacity
    * Virtualization strategy: scale up and down at our capacity 
    * Massive Security problems if two VMS in a server are not seperate from each other. 
    * Example: coca-cola and pepsi shared recipie.txt - fork bomb
    * Performance problems 
        *   Wasting capacity on OS, Hinting at containers here
    * Example: AWS EC2, Digital Ocean


- Public Cloud: GCP, AWS, Azure. 
    * Hard part is they're still just giving you machines, you have to manage all the software networking, provisioning, updating etc
    * Cheaper as there is no need to pay to people to manage physical data center

- Containers give us many of the security and resource-management features of VMs 
   * "Light weight" :) 
   * freezing file system and dumping it to a container image, host OS executes these containers
   * Without the cost of having to run a whole other OS.
