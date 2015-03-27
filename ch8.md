# Error catching
No program is perfect. Even if you could create a perfect program, there will always be that one user that types in a string when you ask for an integer. Then the program crashes. It gets even more difficult in parallel computing with problems such as race conditions.

## Killswitch calls

If you remember from 2.2.5, HIVE uses ports 9000 and 9002-9100. What about 9001? Port 9001 is a UDP port that is constantly listening for messages from any node. If any node hits a fatal error, slave or master, it will broadcast over UDP port 9001 the name of the current program. All nodes that are running that program will terminate immediately and free their resources. This 'killswitch' can also be triggered manually by the user through the HIVE console window.

## Warnings

If the node hits an error, but it thinks it can continue, it declares a **warning**. This is declared through the TCP connection on a port from 9002-9100 to the master node. The master node will display the warning, and details if the verbose option is enabled. Warnings don't neccesarily mean anything is wrong, but some warnings can bring an entire hive down if you aren't careful.