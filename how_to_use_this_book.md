# How to use this book
## 0.1 Structure
This book has been divided down into three parts. The first part will give the reader an overview of the HIVE infrastructure. In addition, it will assist the reader in setting up their testing environment. The second part will take a detailed look at how the underlying engine of HIVE works. This section is intended for people who wish to deal more with the hardware side of running a hive. The final part will provide a reference for programming in HIVIL. It will also show how to port over existing C++ and C# using the HIVEAPI library.

## 0.2 Types of text
Any type of code will be in a grey box with the file name (relative to your project directory) written below. For example:

    // Language: C#
    using System;
    using System.IO;
    
    static int Main()
    {
        // Write our message to the Console stream
        Console.Writeline("Hello World!");
        return 0;
    }
*/Hello.cs*

Commands you may need to type in one line at a time (such as PowerShell commands) will appear in **BOLD**. For example:

&nbsp;&nbsp;&nbsp;&nbsp;To get your IP address, open Command Prompt and enter **ipconfig -all**.

## 0.3 Sections
All headers in this book follow the following convention: PART.SECTION.SUB-SECTION. The SUB-SECTION will be present only when a section needs to be further broken down.