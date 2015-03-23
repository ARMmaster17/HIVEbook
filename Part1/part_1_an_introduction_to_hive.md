# Part 1: An Introduction to HIVE
## 1.1 What is HIVE?
The Oxford Dictionary defines a system as "a set of connected things or parts forming a complex whole". HIVE in itself is a system. It is composed of many "parts" that form a "complex whole". But the HIVE system doesn't need to be complex. In fact, the underlying design principles in the HIVE system were created to overcome this problem. The hardware and networking side is more-or-less isolated from the software side. A hardware engineer can create a hive without knowing what the program will do. Similarly, a software engineer can write a program without having to know anything about the underlying hardware.

Yet, these sub-systems of HIVE can be somewhat complex. Networking protocols and language syntax can be confusing at first glance. But just like any other programming language or network setup, it's only complex as you make it. Take for example an automobile. If you ask any random person off the street to build an automobile from scratch, most would claim it to be impossible. Modern automobiles can contain almost foreign inner-workings with systems such as stereos and automatic transmissions. But at its core level, an automobile is a vehicle with wheels that is powered by an internal engine. Now the aforementioned task seems more feasible.

At its core level, HIVE is a system that runs commands in a script file while distributing the work across multiple computer nodes. From this statement, HIVE can be broken down into two parts: the HIVE engine and the HIVIL scripting component.

### 1.1.2 HIVE Engine
Keeping the analogy of an automobile as a system, the massive network of wires, tubes, and pipes is what the HIVE engine consists of. The HIVE engine is responsible for moving everything from place to place in a seamless manner. It also consists of the electrical system, which is responsible for notifying the user when there in a malfunction, and safely brings the vehicle to a stop (hopefully).

### 1.1.3 HIVIL Scripting
If the HIVE engine is the transport network of an automobile, then HIVIL script is anything that goes through that network. HIVIL scripting is the code you feed into the HIVE engine. The HIVE engine will manage breaking up your program across multiple nodes, so you can just focus on the actual program logic. Like how a car is useless without a fuel supply, the HIVE engine is useless without a HIVIL program to process. But HIVIL doesn't just consist of the contents of the transport network, it also composes of the actual devices at the ends of the strands of the network. HIVIL also includes the interpreter, which is responsible for translating the script commands into native machine code. It also carries out variable manipulations across the network to keep the entire system in sync. It will also temporarily pause the program if a required function is not completed yet.

## 1.2 Setting up your testing environment
### 1.2.1 Development machine
The most simple development environment you can use is any computer with a text editor such as notepad. Yep, that's it. If you would like to get a little more advanced, you may want to look at Notepad++ (http://www.notepad-plus-plus.org) or the official HIVE IDE. At the time of this writing, it is in an unstable state and is not recommended for regular use.

### 1.2.2 Dedicated slave node
All nodes with the HIVE engine can run as slave and master nodes at the same time. The master node is defined as the node from which the script file and variable library is stored. A dedicated slave node will have one of the lowest system requirements. If it can run Windows 7 (x86/32-bit) or better, it's good. For optimum performance, you may want to add an ethernet or even a fiber optic interface card (PCI). 512 MB of RAM will be plenty since most of the file and memory operations happen on the master node.

### 1.2.3 Dedicated master node
This machine will have to be very powerful. The actual requirements will depend on how many slave nodes you have. At least a hex core 64-bit CPU with 8 GB of RAM should be more than plenty for most mid-scale operations. Dual ethernet is not always needed, but should be considered for larger deployments. At least Windows 7 is required, but Windows Server 2012 R2 would be the best choice for deployment. The best way to know your requirements is through trial and error in your testing lab.

### 1.2.4 Dual mode node
If you wish to have nodes that operate as both slaves and nodes, it is recommended that you follow the same guidelines as in 1.2.3. The only exception would be dual ethernet. This is not recommended unless you have a good reason to. For optimum results, all nodes should have the same hardware layout.

### 1.2.5 Network layout
If you think you can get by with Wi-Fi, all I can say is good luck. For a solid network, you will need an all-ethernet network. All nodes should be on the same subnet. VPN across datacenters is possible, but can lead to unpredictable results from network ping. If you are part of an enterprise environment, a dedicated router or a switch will suffice to prevent a DDoS-style knockout of your enterprise network. A firewall is neccesary if your network directly interfaces with other computers over the Internet.