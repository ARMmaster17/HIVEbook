# Setting up your testing environment
## Development machine
The most simple development environment you can use is any computer with a text editor such as notepad. Yep, that's it. If you would like to get a little more advanced, you may want to look at Notepad++ (http://www.notepad-plus-plus.org) or the official HIVE IDE. At the time of this writing, it is in an unstable state and is not recommended for regular use.

## Dedicated slave node
All nodes with the HIVE engine can run as slave and master nodes at the same time. The master node is defined as the node from which the script file and variable library is stored. A dedicated slave node will have one of the lowest system requirements. If it can run Windows 7 (x86/32-bit) or better, it's good. For optimum performance, you may want to add an ethernet or even a fiber optic interface card (PCI). 512 MB of RAM will be plenty since most of the file and memory operations happen on the master node. A general rule of thumb is that your slave node should not be more than half as powerful as your master node.

# Dedicated master node
This machine will have to be very powerful. The actual requirements will depend on how many slave nodes you have. At least a hex core 64-bit CPU with 8 GB of RAM should be more than plenty for most mid-scale operations. Dual Ethernet is not always needed, but should be considered for larger deployments. At least Windows 7 is required, but Windows Server 2012 R2 would be the best choice for deployment. The best way to know your requirements is through trial and error in your testing lab.

## Dual mode node
If you wish to have nodes that operate as both slaves and nodes, it is recommended that you follow the same guidelines as in 1.2.3. The only exception would be dual ethernet. This is not recommended unless you have a good reason to. For optimum results, all nodes should have the same hardware layout.

## Network layout
If you think you can get by with Wi-Fi, all I can say is good luck. For a solid network, you will need an all-ethernet network. All nodes should be on the same subnet. VPN across data centers is possible, but can lead to unpredictable results from network ping (race conditions, discussed in part 3). If you are part of an enterprise environment, a dedicated router or a switch will suffice to prevent a DDoS-style knockout of your enterprise network. A firewall is neccesary if your network directly interfaces with other computers over the Internet.