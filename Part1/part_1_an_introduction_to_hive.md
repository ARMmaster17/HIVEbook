# Part 1: An Introduction to HIVE


## 1.2 Setting up your testing environment
### 1.2.1 Development machine
The most simple development environment you can use is any computer with a text editor such as notepad. Yep, that's it. If you would like to get a little more advanced, you may want to look at Notepad++ (http://www.notepad-plus-plus.org) or the official HIVE IDE. At the time of this writing, it is in an unstable state and is not recommended for regular use.

### 1.2.2 Dedicated slave node
All nodes with the HIVE engine can run as slave and master nodes at the same time. The master node is defined as the node from which the script file and variable library is stored. A dedicated slave node will have one of the lowest system requirements. If it can run Windows 7 (x86/32-bit) or better, it's good. For optimum performance, you may want to add an ethernet or even a fiber optic interface card (PCI). 512 MB of RAM will be plenty since most of the file and memory operations happen on the master node. A general rule of thumb is that your slave node should not be more than half as powerful as your master node.

### 1.2.3 Dedicated master node
This machine will have to be very powerful. The actual requirements will depend on how many slave nodes you have. At least a hex core 64-bit CPU with 8 GB of RAM should be more than plenty for most mid-scale operations. Dual Ethernet is not always needed, but should be considered for larger deployments. At least Windows 7 is required, but Windows Server 2012 R2 would be the best choice for deployment. The best way to know your requirements is through trial and error in your testing lab.

### 1.2.4 Dual mode node
If you wish to have nodes that operate as both slaves and nodes, it is recommended that you follow the same guidelines as in 1.2.3. The only exception would be dual ethernet. This is not recommended unless you have a good reason to. For optimum results, all nodes should have the same hardware layout.

### 1.2.5 Network layout
If you think you can get by with Wi-Fi, all I can say is good luck. For a solid network, you will need an all-ethernet network. All nodes should be on the same subnet. VPN across data centers is possible, but can lead to unpredictable results from network ping (race conditions, discussed in part 3). If you are part of an enterprise environment, a dedicated router or a switch will suffice to prevent a DDoS-style knockout of your enterprise network. A firewall is neccesary if your network directly interfaces with other computers over the Internet.

## 1.3 Installing the HIVE environment
At this point, you should have at least two eliglbe nodes on a wired network of some kind. For each node, you will need to install the HIVE engine service, for this you have a few options.

### 1.3.1 Download files
You will need to aquire the files for HIVE. The free version comes in either a .zip or .tar.gz compressed file. Purchasing the enterprise version also gives the option of a .iso file that can be burned to a CD, DVD, or USB storage device. From there you will need to copy all files to a folder on your computer where an administrator can run the program. Copying the files to a RAID-enabled SAN may be desirable for large-scale deployment.

### 1.3.2 Compile from source
This option is only available for trial purposes or organizations with an enterprise license. You will need Visual Studio 2013. This will be covered later in this part of the book. Keep in mind that directly modifying the HIVE engine is a violation of the license agreement.

### 1.3.4 Deploy from MSI
At this time, there is no official MSI package available. If you have the tools to create MSI packages, you can deploy HIVE on all computers on the network using pre-built setting files. It is also possible to deploy batch scripts alongside the setup files to create settings files specific to each node.

## 1.4 Setting up HIVE
For the remainder of the book, the directory where you installed HIVE will be called the *Application Root*. It will also be represented by the *./* in file names. It is important at this point that you verify that at least the system administrator has read/write/execute rights to the *Application Root* and its contents.

Open up *settings.ini* from the *Application Root*. Since the HIVE downloads are uploaded directly from the Continious Integration software, there should already be a few settings in this file. A simple settings file would look something like this:

    MODFOLDER=C:\*APPLICATION ROOT*\modules
    LANG=en-us
    FS=C:\*APPLICATION ROOT*\filestore
    UNITID=B1F1R1U1
    INTER=-1

*./settings.ini*

For a first run in a lab enviroment, all you would need to do is replace *APPLICATION ROOT* with the actual path to your istallation of HIVE. If you just want to get up and running, copy this to your *./settings.ini* file, then jump to section 1.5. If you need a more andvanced configuration, see below.

### 1.4.1 Config file syntax
#### 1.4.1.1 Comments
If you are familiar with programming, you may understand what a comment is. A comment is a line inside of a file that the interpreter/compiler ignores. The purpose of a comment is to tell the reader, in english, what that line of code does. It helps organize code files and is a great habit to obtain in any branch of computer programming.

To declare a line a comment, start your line like this:

    //
    
After this line, you may put any text you wish. Do note that within config files, HIVE does not support adding comments on the same line as code. This will either cause an error or some **really** interesting results.

#### 1.4.1.2 Keys and values
In a HIVE config file, The text on the left side of the *=* sign is the name of the setting you are modifying/setting. This is known as the **key**. Keys are always in all caps. Anything to the right of the *=* sign is the **value** you are assigning to the key. Table 1.4.1.2A shows a list of all supported keys and suggested values.

*Figure 1.4.1.2-A*

| Key | Value type |Values supported |
| -- | -- | -- |
| UNITID | String | A name to identify the unit in the HIVE. Must be unique from all nodes on the network. |
| INTER | Integer | Number of network interface to use. Put -1 to pick the first non-loopback interface. |
| LANG | String(locale) | Name of the locale used in your country. When in doubt, pick 'en-ALL'. |
| MODFOLDER | String(filepath) | Path to the folder containing extension modules. |
| FS | String(filepath) | Path to a folder where you would like the File Store folder created. Put on an SSD if possible. |

## 1.5 First run

Once you have your config file set up how you like, go ahead and start up the machine. If will be working in a master/slave enviroment, you should start HIVE first on the dedicated master node. In a dual mode enviroment, it does not matter the startup order. For now, just bring one node online.

In the application folder, find hivil.exe and run it. The console windows will then open. Since this is your first time running HIVE, it will need a minute to set up. HIVE will set up the neccesary folders and parse the settings file. The full setup process is described in part 2.

You will know HIVIL is fully started when you see the following command:

    @*UNITID*: 
    
Replace *UNITID* with whatever you set it to in your config file. Now HIVE is awaiting a command. Go ahead and type **hive version**. You should see output similar to this:

    HIVE interpreter and enviroment
    VERSION: 1.0.0 Stable
    UNITID: *UNITID*
    IP: xxx.xxx.x.xxx
    
This command shows some basic system information. The important part is making sure that the *VERSION* line on your computer is the same as the one above. If your version does not read *Stable*, your version of HIVE may be missing some features or have new functions that may have bugs. Be sure to obtain the latest version of HIVE from the website.

Now type in **hive net** and press enter. You should see something similar to this:

    HIVE NETWORK
    INTERFACE: ANY  IP: xxx.xxx.x.xxx
    -------------------------------------
    | UNITID | IP | State | Load | Ping |
    -------------------------------------
    
    
    Connected nodes: 0
    
If you see a computer pop up on the list, someone on your network is running a version of HIVE. If this is not what you intended, you may need to add a firewall to your network.

Now let's get a slave node on the network. Repeat the above steps on another computer (or another virtual machine). When you get to the **@UNITID:** prompt, go back to the master node and type in **hive net** again. Now you should see something like this:

    HIVE NETWORK
    INTERFACE: ANY  IP: xxx.xxx.x.xxx
    -------------------------------------
    | UNITID | IP | State | Load | Ping |
    -------------------------------------
    B1F1R1U2 .xxx   Ready    0      2
    
    Connected nodes: 1
    
This shows that a unit called B1F1R1U2 is on the network, is ready, is running zero tasks, and has a ping of 2 ms. Repeat these steps for any nodes you wish to add.

If you didn't see your node come up, you may have a firewall or a network issue. Like I mentioned at the beginning of this book, I assume that you know how to resolve problems specific to your setup. Remember to try this within a testing lab before you try anything fancy.

## 1.6 Running your first program

Now that you have at least one slave node on your network, you are ready to run your first program. Take a look at your application root. If you have a folder called *./samples*, you may proceed. Otherwise you will need to copy *sample.hive* from part 3 of this book.

Once that is complete, go to the console window and type in **run C:\APPLICATION ROOT\samples\sample.hv** and press enter. This will run the sample program. You should see a continously incrementing number being printed to the console. If you type in **hive net** again, you should see that the load for the node has increased to a 1. Do not worry if the load is higher than 1, this just means that your node is slower than your master node. More information about how the sample program works can be found at the beginning of part 3.