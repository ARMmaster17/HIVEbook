# Part 1: An Introduction to HIVE
## 1.1 What is HIVE?
The Oxford Dictionary defines a system as "a set of connected things or parts forming a complex whole". HIVE in itself is a system. It is composed of many "parts" that form a "complex whole". But the HIVE system doesn't need to be complex. In fact, the underlying design principles in the HIVE system were created to overcome this problem. The hardware and networking side is more-or-less isolated from the software side. A hardware engineer can create a hive without knowing what the program will do. Similarly, a software engineer can write a program without having to know anything about the underlying hardware.

Yet, these sub-systems of HIVE can be somewhat complex. Networking protocols and language syntax can be confusing at first glance. But just like any other programming language or network setup, it's only complex as you make it. Take for example an automobile. If you ask any random person off the street to build an automobile from scratch, most would claim it to be impossible. Modern automobiles can contain almost foreign inner-workings with systems such as stereos and automatic transmissions. But at its core level, an automobile is a vehicle with wheels that is powered by an internal engine. Now the aforementioned task seems more feasable.

At its core level, HIVE is a system that runs commands in a script file while distributing the work across multiple computer nodes. From this statement, HIVE can be broken down into two parts: the HIVE engine and the HIVIL scripting component.

### 1.2 HIVE Engine
Keeping the analogy of an automobile as a system, the massive network of wires, tubes, and pipes is what the HIVE engine consists of. The HIVE engine is responsibe for moving everything from place to place in a seamless manner. It also consists of the electrical system, which is responsible for notifying the user when there in a malfunction, and safely brings the vehicle to a stop (hopefully).

### 1.3 HIVIL Scripting