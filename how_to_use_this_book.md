# How to use this book
## 0.1 Structure
This book has been divided down into three parts. The first part will give the reader an overview of the HIVE infrastructure. In addition, it will assist the reader in setting up their testing environment.

The second part will take a detailed look at how the underlying engine of HIVE works. This section is intended for people who wish to deal more with the hardware side of running a hive.

The final part will provide a reference for programming in HIVIL. It will also cover how to port over existing C++ and C# applications via the API library.

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
*./Hello.cs*

Commands you may need to type in one line at a time (such as PowerShell commands) will appear in **BOLD**. For example:

&nbsp;&nbsp;&nbsp;&nbsp;To get your IP address, open Command Prompt and enter **ipconfig -all**.

## 0.3 Sections
All headers in this book follow the following convention: PART.SECTION.SUB-SECTION. The SUB-SECTION will be present only when a section needs to be further broken down.

## 0.4 Assumptions made by the author
You don't need a PHD in Computer Science to learn HIVE, but there are some things I assume you already know.

### 0.4.1 Software
In order to keep this book short, I assume that you already have a working knoledge of programming language design. No specific language experience is required, but you should understand program logic, code flow, and basic data structures.

If you lack this experience, I reccommend you read up on the BASIC programming language. Syntax-wise, it will be very similar. Users wishing to take advantage of the API must have a strong background in C++ or C#.

### 0.4.2 Hardware
For the sake of simplicity, I assume you know what an ethernet cable is and how to use it. Joking aside, you should have a good idea of how to design an enterprise-level network. In additon, you should understand the concept of latency (or lag to put it in gaming terms) and how to optimize ping time.

I also assume that you know how to set up a test-bed network, should it be through Hyper-V or physical machines. I also assume you know how to walk yourself through a Windows Server installation. I will briefly gloss over MSI installation, but it may be worth it to take a more in-depth look at large-scale MSI deployment.