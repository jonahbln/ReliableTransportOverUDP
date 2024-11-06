Reliable Transport - README
- High-Level Approach:
This is a TCP-like reliable transport protocol written overtop of UDP in a simulated sender-reciever system. We adopt many techniques that TCP does to ensure consisten, recverable transport of data between the sender and reciever which should work even in unreliable and unstable network environments.

Breakdown of the Program Flow -
1. Initialization:
The Sender starts by setting congestion control variables (like congestion window and slow start threshold) and it establishes a UDP socket for communication with the reciever.
The Receiver binds a UDP socket to listen for incoming packets and track sequence numbers.

2. Sending Data:

The Sender reads data from stdin and packs it into individual packets with monotonically incremented sequence numbers. Each packet is sent to the receiver, and an entry is added in the sender's packetsAwaitingResponse list which tracks packets without acknowledgments.

3. Receiving and Acknowledging Data:

The Receiver accepts packets as they come in, verifies if they come in the right order, and sends ACKs back to the sender for each received packet including the seq number.
Received packets are stored, and out-of-order packets are buffered until prior packets arrive and can be patched together.

4. Congestion Control:

When it receives ACKs, the Sender adjusts the congestion window (cwnd) based on the TCP slow start and congestion avoidance phases.
The Sender also resends packets that exceed a set timeout threshold which is 2 times the estimated round trip time of a packet to handle lost packets.

5. Closure:

Once all data has been transmitted and acknowledged, the Sender exits gracefully.

- Challenges Faced:
Congestion Control in an Unreliable Environment:
Implementing a basic TCP-like congestion control algorithm required tricky tuning of fields like cwnd and ssthresh to avoid overwhelming the receiver or network.

Out-of-Order Packet Handling:
The receiver needed logic to buffer out-of-order packets while waiting for missing packets to maintain the correct sequence.

Resending lost packets:
It was tricky figuring out how to determine when a packet is lost, and how to estimate round trip time on the sender side.

Testing Approach -
- The router was thoroughly tested using various configuration scenarios (provided by CS4700 professors) to ensure its functionality and correctness.
- These scenarios included testing for packet loss, duplication, and varying network delays to validate protocol functionality and ensure robust handling under unreliable network conditions.