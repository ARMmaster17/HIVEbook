


## 3.2 C# API library



## 3.3 Programmers reference
### 3.3.1 HIVIL

#### 3.3.1.1 Directives

##### #START:[string(jobname)]
Tells the interpreter which job to run in elevated slave mode.

##### #NAME:[string]
The name of the program. Used for killswitch broadcasts.

##### #DESC:[string]
Description of the program. Not required.

##### #HIVE:[version]
Minimum version of HIVE required to run program.

#### 3.3.1.2 Math

##### ADD [int a] [int b]
Adds two integers, and assigns the value to int b.

##### ADD [int a] [int b] [int c]
Adds integers a and b, then assigns the value to int c.

##### SET [var] [value]
Assigns the key [var] the value [value].

##### SUB [int a] [int b]
Subtracts [int b] from [int a], then assigns the value to [int b].

##### SUB [int a] [int b] [int c]
Subtracts [int b] from [int a], then assigns the value to [int c].

#### 3.3.1.3 Task management

##### RUN [string(jobname)]
Runs the specified job on a new slave node.

##### RUN [string(jobname)] [int a]
Runs the specifed job on a new slave node and assigns the process UID to [int a]

#### 3.3.1.3 Variable declarations

##### HIVEINT [string(variablename)]
HIVE extension of the c-sytle *int* type.

##### HIVEBOOL [string(variablename)]
HIVE extension of the c-sytle *bool* type.

##### HIVESTRING [string(variablename)]
HIVE extension of the c-sytle *char[]* type.

### 3.3.2 HIVE API