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

If you have done any amount of programming before, you probably know what a comment is. A comment is a line that is ignored by the parser. Comments are used to help you identify sections of code and to keep your code organized. Every programmer **SHOULD** make it a habit to comment their code as much as possible.

## 3.2 C# API library



## 3.3 Programmers reference
### 3.3.1 HIVIL

#### 3.3.1.1 Directives

#### 3.3.1.2 Math

*ADD [int a] [int b]*

Adds two integers, and assigns the value to int b.

*ADD [int a] [int b] [int c]*

Adds integers a and b, then assigns the value to int c.

*SET [var] [value]*

Assigns the key [var] the value [value].

*SUB [int a] [int b]*

Subtracts [int b] from [int a], then assigns the value to [int b].

*SUB [int a] [int b] [int c]*

Subtracts [int b] from [int a], then assigns the value to [int c].

#### 3.3.1.3 Task management

### 3.3.2 HIVE API