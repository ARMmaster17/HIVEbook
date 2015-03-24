# Part 3: Coding for HIVE
This part is broken up into three sections. The first section will walk you through the sample programs. The second section will show you how to use the C# library in your existing applications. The final section will give a full reference to the HIVIL language and the HIVE API.
## 3.1 Sample program

If you chose to comile HIVE from source, you may have noticed a folder called *./samples*. This folder contains sample programs for you to examine.

*If you didn't compile from source, I have included a slimmed-down version of each program*

### 3.1.2 *sample.hive*
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

#### 3.1.2.1 Directives
Any line that starts with # is marked as a directive. A directive is a special command that is interpreted by the parser. For example, with line 1 we see the following code:

    #HIVE:1.0
    
This tells the parser to add a special attribute to the final program. This attribute will notify the node that they must have HIVE version 1.0 or greater to run this program.

Directives are ALWAYS placed at the top of the program and are always followed by a blank line. This tells the parser that we are done with directives and we are ready to start reading actual program code.

The next two directives worth noting are *#NAME* and *#DESC*. *#NAME* tells the parser the name of the program. This is important for killswitch calls (see 2.3.1). For this reason it is important that you give your HIVE program a unique name. The *#DESC* directive isn't required, but it can be useful for knowing what a program does without looking at the code.

The last directive in this sample program is the *#START* directive. This tells the interpreter where to start. The job name listed here is the only job that is allowed to run in elevated slave mode. This means that the job is allowed to interface with the user through the console. For a more in depth look at elevated slave mode, see section 2.2.4.

#### 3.1.2.2 Comments

If you have done any amount of programming before, you probably know what a comment is. A comment is a line that will be ignored by the parser. Comments are used to help you identify sections of code and to keep your code organized. Every programmer **SHOULD** make it a habit to comment their code as much as possible.

#### 3.1.2.3 Job descriptors

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

#### 3.1.2.4 Variables

I'm going to skip around a bit within the program to keep this book organized. Take a look at the following lines in the sample program:

    ...
    HIVEINT adder
    ...
    INT one
    ...
    
These lines declare a variable. As I mentioned before, by reading this book, I assume you know what a variable is and understand the basic C-style data structures (int, char[], bool). From that I assume you already know what the second line does.

The first command is where the HIVE engine becomes involved. Unlike an int, which is stored in RAM on the local computer, a HIVEINT is stored in the varstore on the master node. HIVEVARS *must* be declared within the job defined in **#START**. Consider a HIVEVAR to be a sub-type of its C-style var type.

#### 3.1.2.5 Loops

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

## 3.2 C# API library



## 3.3 Programmers reference
### 3.3.1 HIVIL

#### 3.3.1.1 Directives

**#START:[string(jobname)]**

Tells the interpreter which job to run in elevated slave mode.

**#NAME:[string]**

The name of the program. Used for killswitch broadcasts.

**#DESC:[string]**

Description of the program. Not required.

**#HIVE:[version]**

Minimum version of HIVE required to run program.

#### 3.3.1.2 Math

**ADD [int a] [int b]**

Adds two integers, and assigns the value to int b.

**ADD [int a] [int b] [int c]**

Adds integers a and b, then assigns the value to int c.

**SET [var] [value]**

Assigns the key [var] the value [value].

**SUB [int a] [int b]**

Subtracts [int b] from [int a], then assigns the value to [int b].

**SUB [int a] [int b] [int c]**

Subtracts [int b] from [int a], then assigns the value to [int c].

#### 3.3.1.3 Task management

**RUN [string(jobname)]**

Runs the specified job on a new slave node.

**RUN [string(jobname)] [int a]**

Runs the specifed job on a new slave node and assigns the process UID to [int a]

### 3.3.2 HIVE API