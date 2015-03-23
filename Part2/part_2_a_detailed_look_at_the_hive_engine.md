# Part 2: A Detailed Look At The HIVE Engine
## 2.1 Introduction
This section is intended for system engineers who will be dealing with the hardware side of operating a hive. Software developers may skip this section if they wish. HIVE is developed so that programmers don't have to know how the hardware works, they just need to know that it works. The reverse is also true. However it may be useful to know how the underlying HIVE engine works when you run into issues like race conditions, which are common in multi-threaded or distributed computing.
### 2.1.1 Foolish assumptions
By reading this part of the book, I assume that you have a strong background in the following:
* Enterprise networking
* System provisioning
* Hyper-V/virtualization (depends on your needs)
* RAM and SWAP space
* Latency and ping
* File permissions in NTFS

Unfortunately, unlike the programming side, there is not much help I can offer if you are lacking one of the above skill sets. When it comes to hardware setups, there is no replacement for real-world experience. A book can teach you about individual parts of a computer system, but only experience will teach you how to mix those concepts into working ideas. If you have ever dealt with a cluster running MPI or similar software, that qualifies too.

I also assume that you have already gone through Part 1 of this book. That includes following along with the examples on your own hardware. If you don't already have a functional HIVE setup, I suggest you take care of that now. Learning isn't a spectator sport.

## 2.2 Cycle of a HIVE program
All HIVE programs go through the same cycle (except in the event of a fatal crash). Below we'll walk through the lifecycle of a HIVE application.
### 2.2.1 Background
Before the program even starts, the HIVE service must be running in the background. The HIVE service does two things: waits for a task from a master node, and waits for a task from the user.

When it waits for a task, it is constantly sending its UNITID over UDP. Any node running HIVE will recieve the message and store it in its list of available nodes. If a unit does not respond for over a specified number of seconds, it is removed from the index. This timeout can be set in the settings file along with its UNITID.

### 2.2.2 Trigger

When the HIVE service starts up, it shows a console window. This window shows status messages and prompts the user for a command. If the program is compiled with the following preprocessor directive:

    #DEFINE DEBUG
    
Then detailed messages will be shown, along with data about UDP broadcasts. Because of the high amount of console writes, it is recommended that you not do this unless you are fixing bugs withing the confines of your own testing network.

To run a program, the user simply types in the following command:

    @UNITID: RUN C:\path\to\program\
    
Unlike other enviroments, you will need to point hive to a folder containing the program files. When a programmer sends their script file to the parser, the file is split into 'jobs'. Each job is a separate file that will be sent to different nodes. For more information, see Part 3 of this book about HIVIL.

Once the HIVE engine is pointed to a program directory to run, it creates a new thread. This way it can keep itself available to run tasks as a slave and process commands from the user.

### 2.2.3 Setup

Once the new thread is started and memory is allocated, it loads the first program into memory. HIVE will look for a program file called main.hv. The next step is performed by the parser. The parser will look through the header of the file and checks the version number. It makes sure that the currently installed version of HIVE can run the program. Next it copies the list of nodes in the hive, and deletes any that do not have a sufficient version number.

Before running, it also sets up a varstore. This varstore is a high-speed database that will store vars that need to be used across nodes.

### 2.2.4 Running

Once all the checks have passed, it begins runnng the program. It runs as if it is a slave node, but functions like **print** and other interfacing functions are available. This is called an **elevated slave**. It will begin stepping through the **main** function. Any time a HIVEVAR is declared, it will add it to the varstore. It is entirely possible to write a program that just runs on the master node, but this defeats the purpose of using HIVE. You might as well just go back to regular programming. Whenever the code hits a call similar to this:

    RUN job1
    
It will prepare to send *job1* to a slave node.

### 2.2.5 Calling the slave node

First what will happen is the HIVE engine will look through the list of available nodes. It will pick the node with the lowest ping and/or current CPU usage. The master node will connect through TCP over port 9000 to initiate a connection. The slave node will then respond with an open port. The range of ports to be used can be defined in *./settings.ini*, but default to ports 9002-9100.

Once the master node recieves a port number, it will initiate a file transfer through that port. The job file will temporarily be downloaded to the File Store folder specified in the settings file.

### 2.2.6 Running the job concurrently

Once the file initiation is complete, the slave node will begin interpreting and executing the .hv file it downloaded.

Whenever the interpreter finishes executing a line of script, it will tell the master node. This is a failsafe in case the node goes down. Should a node go down due to a power outage or a system reset, the master node can just send the job to a new node with the specification to start off on the line that the other node died on.

Any time a call for a HIVEVAR is made, the HIVE API will make a call to the master node's varstore. It will return with the result and continue execution.

Upon finishing a job, the slave node will terminate the thread, and delete all temporary files in the filestore.

### 2.2.7 Cleaning up

When the master node is finished executing the program, it returns the result and closes the thread. All network connections are closed and RAM/SWAP resources are released.

## 2.3 Error catching
No program is perfect. Even if you could create a perfect program, there will always be that one user that types in a string when you ask for an integer. Then the program crashes. It gets even more difficult in parallel computing with problems such as race conditions.

### 2.3.1 Killswitch

If you remember from 2.2.5, I said that HIVE uses ports 9000 and 9002-9100. What about 9001? Port 9001 is a UDP port that is constantly listening for messages from any node. If any node hits a fatal error, slave or master, it will broadcast over UDP port 9001 the name of the current program. All nodes that are running that program will terminate immediately and free their resources. This 'killswitch' can also be triggered manually by the user through the HIVE console window.

### 2.3.2 Warnings

If the node hits an error, but it thinks it can continue, it declares a **warning**. This is declared through the TCP connection on a port from 9002-9100 to the master node. The master node will display the warning, and details if the verbose option is enabled. Warnings don't neccesarily mean anything is wrong, but some warnings can bring an entire hive down if you aren't careful.