# Part 1: An Introduction to HIVE








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