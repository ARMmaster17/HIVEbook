# Sample programs

If you chose to compile HIVE from source, you may have noticed a folder called *./samples*. This folder contains sample programs for you to examine.

*If you didn't compile from source, I have included a slimmed-down version of each program*

## Compiling programs

When you write a HIVE program, you write it into a .hive file. This is done through any text editor from Notepad to Visual Studio. However, you cannot run the .hive file, you have to parse it first.

Each HIVE program will need it's own directory. Create a directory that looks like this:

    testapp
        testapp.hive

Now open *testapp.hive* using any text editor of your choice. Paste the following code into it:

    #HIVE:1.0
    #NAME:testapp
    #START:main
    
    JOB main
    RUN test1
    PAUSE
    HALT
    ENDJOB
    JOB test1
    REPEAT
    ENDREPEAT
    ENDJOB

Don't worry if the code looks a little strange, we'll cover that later. Basically, this program will start a new job which will infinitely loop, then quit. This is all we need to show how the parser works.

Now open up HIVE if it isn't already open, and type the following in:

    pc PATH_TO_PROGRAM.hive

Now give it a minute to parse the file. This could take a very long time if you have a slow computer or a large program. Once you get back to the prompt, open up the folder containing your HIVE program. You should now see something like this:

    testapp
        testapp.hive
        varstore.v
        main.hv
        test1.hv
        data.v

Now let's figure out what exactly we are looking at. It is worth noting that it is usually a very bad idea to edit parsed files. It is much safer to edit the .hive file and recompile.

The first file is the .hive file you created earlier, no suprises there. The next file is *varstore.v*. This is a collection of all the HIVEVARS that will be used in the program. This helps speed up the reading and writing to the varstore.

The next two files both end in .hv. When the parser goes through the HIVE program, it splits each job into its own file. This allows the master node to simply send a file to slave nodes, which is much faster than trying to find its location in a file.

The last file is *data.v*. This file stores data about how HIVE should handle your program. It also contains information from the directives at the top of your progrm (anything that starts with *#*).

It is important to know that compiled programs **only** work with the version they were compiled with. For example, if you compiled a program with HIVE version 1.3.2, then you can only run the program with HIVE version 1.3.2. To make your programs work with a newer version of HIVE, simply re-parse them with the new compiler.

## *sample.hive*
Within the samples folder, you should see a file called *sample.hive*. Go ahead an open it using your favorite text editor. You should see something like this:

    #HIVE:1.0
    #NAME:sampleprogram
    #START:main
    #DESC:Adds numbers until program termination
    
    // Entry point for the program
    JOB main
        HIVEINT adder
        REPEAT
        RUN compute
        OUTPUT adder
        ENDREPREAT
        PAUSE
        HALT
    ENDJOB
    
    // Job for slave node
    JOB compute
        INT one
        SET one 1
        ADD adder one adder
    ENDJOB
    
This comprises of the first sample program. Now let's dissect it line by line to see what it actually does.

### Directives
Any line that starts with # is marked as a directive. A directive is a special command that is interpreted by the parser. For example, with line 1 we see the following code:

    #HIVE:1.0
    
This tells the parser to add a special attribute to the final program. This attribute will notify the node that they must have HIVE version 1.0 or greater to run this program.

Directives are ALWAYS placed at the top of the program and are always followed by a blank line. This tells the parser that we are done with directives and we are ready to start reading actual program code.

The next two directives worth noting are *#NAME* and *#DESC*. *#NAME* tells the parser the name of the program. This is important for killswitch calls (see 2.3.1). For this reason it is important that you give your HIVE program a unique name. The *#DESC* directive isn't required, but it can be useful for knowing what a program does without looking at the code.

The last directive in this sample program is the *#START* directive. This tells the interpreter where to start. The job name listed here is the only job that is allowed to run in elevated slave mode. This means that the job is allowed to interface with the user through the console. For a more in depth look at elevated slave mode, see section 2.2.4.

### Comments

If you have done any amount of programming before, you probably know what a comment is. A comment is a line that will be ignored by the parser. Comments are used to help you identify sections of code and to keep your code organized. Every programmer **SHOULD** make it a habit to comment their code as much as possible.

### Job descriptors

Moving on, take a look at the following code from the example:

    JOB main
        ...
    ENJOB

This declares a job. A job is a chunk of code that can be run independently from other jobs. Syntax wise, this is the same as the following C# code.

    static void main()
    {
        ...
    }
    
All jobs are static because a job is neither an object nor a member of an object (at least within the scope of this book). It is also void since jobs cannot return a value. Arguments are not currently supported.

This specific job is special. Recall the directive at the top of the program from earlier.

    #START:main
    
This tells the interpreter to start the job *main* on the master node in elevated slave mode. Operating in elevated slave mode allows the job to interface with the user. More on that later.

### Variables

I'm going to skip around a bit within the program to keep this book organized. Take a look at the following lines in the sample program:

    ...
    HIVEINT adder
    ...
    INT one
    ...
    
These lines declare a variable. As I mentioned before, by reading this book, I assume you know what a variable is and understand the basic C-style data structures (int, char[], bool). From that I assume you already know what the second line does.

The first command is where the HIVE engine becomes involved. Unlike an int, which is stored in RAM on the local computer, a HIVEINT is stored in the varstore on the master node. HIVEVARS *must* be declared within the job defined in **#START**. Consider a HIVEVAR to be a sub-type of its C-style var type.

### Loops

Within the sample program, we find the following code:

    REPEAT
    ...
    ENDREPEAT
    
This creates an infinite loop. Rewritten in C#, it would look something like this:

    while(true)
    {
        ...
    }

For our puposes of incrementing an integer until program termination, this works great. However, it may not be desireable in all applications. You may want your program to loop only a certain number of times. In C# or C++, you may have seen a line of code like this:

    break;
    
This tells the program to break out of the current loop. In hive, you do something very similar:

    BREAKREPEAT
    
Unlike the generic **break;**, the derivattive in HIVIL allows you to specify the specific loop you would like to break from, allowing complex nesting appllications.

### Starting a new job

Despite the simplistic syntax, this can be the most frusturating part of programming for HIVE. Take a look at the following line in the sample program:

    RUN compute
    
**RUN** actually has overrides that can be quite useful, but this is the easiest one to use. When this is called, the master node will find the file containing the job, and send it to a new slave node. The slave node will allocate resources and begin interpreting the file. Upon finishing, it will notify the master node, then procced to delete the file and free system resources back to the OS. It is worth noting that in most circumstances, it is usually best to only use the **RUN** call while in elevated slave mode.

#### Problems with parallel programming

Programming multi-threaded or distributed programs can be difficult to debug. Here I'll cover some of these issues that you may come across in these sample programs.

##### Race conditions

Take a look at the following C# program. Assume that any method that ends in **Async** runs on a separate thread. Also assume that editing objects across threads is allowed.

    using System;
    using System.IO;
    using System.Threading;
    
    static void main()
    {
        int a = 5;
        int b = 3;
        int c = 20;
        
        SubtractAsync(a, b);
        SubtractAsync(b, a);
        Console.WriteLine(c);
    }
    
    static void SubtractAsync(int x, int y)
    {
        c = x - y;
    }

Now what if I told you there are three possible outcomes to running this program? It's true! Let's go through the steps that lead to each one.

***Console output: -2***

To get this output, everything would execute in the intended order. First *c* would be set to 2 from the first call to *SubtractAsync()*. Then *c* would be set to -2. Finally the result would be displayed. In a perfect world, this is how the program is intended to run. But once we take into account the problems of asyncronous programming, what seems like math errors, turn into complex race conditions.

***Console output: 2***

Now go back to the first *SubtractAsync()* call. Let's say the first command ran just fine, and *c* is set to 2. Now let's say the second *SubtractAsync()* call is made, but it does not finish in time for the third statement to be run. In this case, *c* is equal to 2 at the time it is called, but it could be set to the intended answer of -2 long after we requested it. This is the first example of a race condition, where two processes are 'racing' to affect the outcome of a program.

***Console output: 20***

Now go back to the beginning and begin executing the program, but assume that **neither** thread returns a value before we print the value of *c* to the console. In this case, *c* would still be at its initialized value of 20. In fact, it could also be changed to either 2 **or** -2 after we already requested the value of *c*. As you can see, race conditions can be quite complex. Things like networking errors and task scheduling made by the operating system can completely change the outcome of a HIVE program.

##### Cross-thread variable manipulation

Many programming languages do not allow editing variables or objects across threads. HIVE does (since jobs are just threads that run on another machine). In order to try and reduce errors that occur from race conditions, HIVE uses a FIFO stack to read an write to the varstore. FIFO stands for First In First Out. This helps prevent problems that occur from reading from and writing to a variable at the same time. Unfortunately, this means that this type of race condition is completely dependent on the stability of your network.

From the example in 3.1.2.6.1, what if the variable *c* was a HIVEINT? This means that variable reading and writing is a network operation. A node missing its turn in a "round robin" style network switch could affect the outcome of the program.

### Closing the program

At the end of the *main* job, we see the following lines:

    ...
    PAUSE
    HALT
    
Although this code is unreachable due to the loop, we still add it for demonstration purposes.

The *PAUSE* function allows the job to temporarily pause execution and give the user a chance to see the program output. Consider the following C# program.

    using System;
    using System.IO;
    
    static int Main()
    {
        Console.WriteLine("Hello world!");
        return 0;
    }

If you were to run this program, the console window would close before you had a chance to read the output. Now consider this new program:

    using System;
    using System.IO;
    
    static int Main()
    {
        Console.WriteLine("Hello world!");
        //////////////////////////////////
        Console.WriteLine("Press enter to continue...");
        Console.ReadLine();
        //////////////////////////////////
        return 0;
    }

Now the program will prompt the user to press enter before continuing. This allows the user to see any output, or to wait for when the user is ready to continue (reconnect a network interface, remove a hard disk, etc...).

The last command is *HALT*. This tells the interpreter to stop running the program. It will broadcast a 'soft killswitch' to all nodes on the network. This is like the aforementioned killswitch call, but it allows the program to come to a safe stop, which is less resources-intensive than a hard killswitch call. Then the temporary files are deleted and the server resources are freed for other programs on the server.

### Math

We aren't done yet, there's still a few lines of code left. Take a look at the *compute* job.

    // Job for slave node
    JOB compute
        INT one
        SET one 1
        ADD adder one adder
    ENDJOB
    
When the parser compiles your program, this job will be placed in a separate file called compute.hv. Upon calling this job, a copy of the job file will be transfered over the network and the slave node will begin execution.

#### Native variable types

In addition to HIVEVARS, HIVE also supports many c-style data structures. A full list of supported types is in the programmers reference section. Take a look at the following line:

    INT one
    
This is no different than either the following C# initialization statements.

    int one = new int(0);
    int one;
    
This line creates a variable with the specified name, then assigns it the default value for that type (again, covered in reference section).

#### Variable manipulation

Once you create a variable, most likely you will want to set it to a value other than its default. This is where you use the *SET* command. Take a look at the following line:

    SET one 1
    
This gives the key *one* a value of '1'. This is similar to either statement in the following C# code:

    one = 1;
    one = new int(1);

#### Math functions

Now take a look at the last line:

    ADD adder one adder
    
Recall from earlier the HIVEINT *adder* from the *main* job. This function will add the values of *adder* and *one*, then assign the value to *adder*. Note that HIVE does not support actual values. The following is not valid HIVE syntax:

    // Invalid HIVE syntax
    ADD adder 1 adder
    
HIVE also support functions other than adding. There are also overloads for the *ADD* call. See the reference section for more information.

## *sample2.hive*

Now that you understand the basics, let's take a look at a slightly more advanced program. This one will take advantage of HIVEs IO library. Take a look at the following program:

    #HIVE:1.0
    #NAME:sample2
    #DESC:Writes 'hello world' to a file
    #START:main
    
    JOB main
        INT processID
        RUN files processID
        AWAIT processID
        PAUSE
        HALT
    ENDJOB
    
    JOB files
        FILE_CREATE test.txt
        FILE_WRITE test.txt "Hello world!"
    ENDJOB
    
This program has some new IO commands, and the *AWAIT* operator. First let's take a look at the IO commands.

### IO operations

Consider the following PHP program:

    <?php
        $fh = fopen('test.txt' 'w');
        fwrite($fh, 'Hello world!');
        fclose($fh);
    ?>

I'll explain why I chose PHP instead of C# in a minute. First take a look at the job *files* and the PHP code. These two functions do the exact same thing. *FILE_CREATE* does the same thing as *fopen()* in PHP. It will create a new file with the specified name. The only difference is that the permissions argument is not required in HIVE.

*fwrite()* and *FILE_WRITE* do the same thing. However, in PHP you must have a file handle to use. This file handle points the command to an IO stream. HIVE automatically opens and closes the stream for each command to free system resources. That is also why there is no HIVE implementation of *fclose()*.

It is important to note that all IO operations take place on the ***master node***. This is to avoid problems with files being spread across an entire cluster. Writing the files to the slave node is not currently supported.

### AWAIT and process IDs

Take a look at the following lines in the HIVE sample program:

    RUN files processID
    AWAIT processID
    
Recall the sample program from 3.1.2:

    RUN compute
    
Many HIVE calls have overrides. This is one such example. Not only are we giving the name of the job to start, but also a pointer to an int. RUN will give the supplied variable the process ID of the job. This allows us to track the job as it executes. Once we execute the *RUN* command, now we can use the process ID in the next line:

    AWAIT processID
    
If you have ever done multi-threaded programming, you may recognize the *await* operator. This operator temporarily halts program execution and waits for the specified job to either finish or crash. Then it continues execution. This is important as we want don't want to inturrupt an IO operation.