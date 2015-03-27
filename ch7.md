# Cycle of a HIVE program
All HIVE programs go through the same cycle (except in the event of a fatal crash). Below we'll walk through the lifecycle of a HIVE application.
## Background
Before the program even starts, the HIVE service must be running in the background. The HIVE service does two things: waits for a task from a master node, and waits for a task from the user.

When it waits for a task, it is constantly sending its UNITID over UDP. Any node running HIVE will recieve the message and store it in its list of available nodes. If a unit does not respond for over a specified number of seconds, it is removed from the index. This timeout can be set in the settings file along with its UNITID.

## Trigger

When the HIVE service starts up, it shows a console window. This window shows status messages and prompts the user for a command. If the program is compiled with the following preprocessor directive:

    #DEFINE DEBUG
    
Then detailed messages will be shown, along with data about UDP broadcasts. Because of the high amount of console writes, it is recommended that you not do this unless you are fixing bugs withing the confines of your own testing network.

To run a program, the user simply types in the following command:

    @UNITID: RUN C:\path\to\program\
    
Unlike other enviroments, you will need to point hive to a folder containing the program files. When a programmer sends their script file to the parser, the file is split into 'jobs'. Each job is a separate file that will be sent to different nodes. For more information, see Part 3 of this book about HIVIL.

Once the HIVE engine is pointed to a program directory to run, it creates a new thread. This way it can keep itself available to run tasks as a slave and process commands from the user.

## Setup

Once the new thread is started and memory is allocated, it loads the first program into memory. HIVE will look for a program file called main.hv. The next step is performed by the parser. The parser will look through the header of the file and checks the version number. It makes sure that the currently installed version of HIVE can run the program. Next it copies the list of nodes in the hive, and deletes any that do not have a sufficient version number.

Before running, it also sets up a varstore. This varstore is a high-speed database that will store vars that need to be used across nodes.

## Running

Once all the checks have passed, it begins runnng the program. It runs as if it is a slave node, but functions like **print** and other interfacing functions are available. This is called an **elevated slave**. It will begin stepping through the **main** function. Any time a HIVEVAR is declared, it will add it to the varstore. It is entirely possible to write a program that just runs on the master node, but this defeats the purpose of using HIVE. You might as well just go back to regular programming. Whenever the code hits a call similar to this:

    RUN job1
    
It will prepare to send *job1* to a slave node.

## Calling the slave node

First what will happen is the HIVE engine will look through the list of available nodes. It will pick the node with the lowest ping and/or current CPU usage. The master node will connect through TCP over port 9000 to initiate a connection. The slave node will then respond with an open port. The range of ports to be used can be defined in *./settings.ini*, but default to ports 9002-9100.

Once the master node recieves a port number, it will initiate a file transfer through that port. The job file will temporarily be downloaded to the File Store folder specified in the settings file.

## Running the job concurrently

Once the file initiation is complete, the slave node will begin interpreting and executing the .hv file it downloaded.

Whenever the interpreter finishes executing a line of script, it will tell the master node. This is a failsafe in case the node goes down. Should a node go down due to a power outage or a system reset, the master node can just send the job to a new node with the specification to start off on the line that the other node died on.

Any time a call for a HIVEVAR is made, the HIVE API will make a call to the master node's varstore. It will return with the result and continue execution.

Upon finishing a job, the slave node will terminate the thread, and delete all temporary files in the filestore.

## Cleaning up

When the master node is finished executing the program, it returns the result and closes the thread. All network connections are closed and RAM/SWAP resources are released.