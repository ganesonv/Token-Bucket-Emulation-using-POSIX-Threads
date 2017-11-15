# Token-Bucket-Emulation-using-POSIX-Threads
(Multi-Threaded programming in C)
Emulated a traffic shaper to transmit packets by a token bucket filter using multi-threading with a single process

The token bucket has a capacity (bucket depth) of B tokens. Tokens arrive into the token bucket according to an unusual arrival process where the inter-arrival time between two consecutive tokens is 1/r. The token arrival rate (the tokens arrive at a constant rate of r). Extra tokens (overflow) would simply disappear if the token bucket is full. A token bucket, together with its control mechanism, is referred to as a token bucket filter.

Four threads where used in this program:
1. Packet thread
2. Token Thread
3. Server1 Thread
4. Server2 Thread

Packets arrive at the token bucket filter according to an unusual arrival process where the inter-arrival time between two consecutive packets is 1/lambda. Lambda the packet arrival rate (The packets arrive at a constant rate of lambda). Each packet requires P tokens in order for it to be eligiable for transmission. When a packet arrives, if Q1 is not empty, it will just get queued onto the Q1 facility. Otherwise, it will check if the token bucket has P or more tokens in it. If the token bucket has P or more tokens in it, P tokens will be removed from the token bucket and the packet will join the Q2 facility, and wake up the servers in case they are sleeping. If the token bucket does not have enough tokens, the packet gets queued into the Q1 facility. Finally, if the number of tokens required by a packet is larget than the bucket depth, the packet must be dropped (otherwise, it will block all other packets that follow it).

The transmission facility (denoted as S1 and S2 are referred to as the "servers") serves packets in Q2 in the first-come-first-served order and at a service rate of mu per second. When a server becomes available, it will dequeue the first packet from Q2 and start transmitting the packet. When a packet has received 1/mu seconds of service, it leaves the system.

When a token arrives at the token bucket, it will add a token into the token bucket. If the bucket is already full, the token will be lost. It will then check to see if Q1 is empty. If Q1 is not empty, it will see if there is enough tokens to make the packet at the head of Q1 be eligiable for transmission (packets in Q1 in also served in the first-come-first-served order). If it does, it will remove the corresponding number of tokens from the token bucket, remove that packet from Q1 and move it into Q2, and wake up the servers in case they are sleeping. It will then check the packet that is now at the head of Q1 to see if it's also eligiable for transmission, and so on.

This system can run in only one of two modes.

Deterministic	 : 	In this mode, all inter-arrival times are equal to 1/lambda seconds, all packets require exactly P tokens, and all service times are equal to 1/mu seconds (all rounded to the nearest millisecond). If 1/lambda is greater than 10 seconds, inter-arrival time of 10 seconds. If 1/mu is greater than 10 seconds, service time is of 10 seconds.
 
Trace-driven	 : 	In this mode, the emulation uses a trace specification file. Each line in the trace file specifies the inter-arrival time of a packet, the number of tokens it needs in order for it to be eligiable for transmission, and its service time.


![Alt text](https://user-images.githubusercontent.com/25571819/32829981-9b664f76-c9a8-11e7-8ae8-576904d05193.png "Token Bucket Filter")
