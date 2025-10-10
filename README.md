# Cycle-2-NS2-Experiment-2
PERFORMANCE ANALYSIS OF THE NETWORK WITH CSMA/CD
# NS2 Simulation: CSMA/CD Network Performance

## 🎯 AIM
To write an NS2 program to observe the performance of the network with Carrier Sense Multiple Access/Collision Detection (CSMA/CD).

## 🧰 EQUIPMENT REQUIRED
- PC System with Linux OS  
- NS2 software

## 🧪 ALGORITHM

1. Start the program.  
2. Declare the global variable `ns` for creating a new simulator.  
3. Set the color for packets.  
4. Open the network animator file in write mode.  
5. Open the trace file and the win file in write mode.  
6. Transfer the packets in the network.  
7. Create the required number of nodes.  
8. Create duplex links between nodes with delay time, bandwidth, and queue mechanism.  
9. Assign positions for the links between nodes.  
10. Set a TCP connection for the source node.  
11. Set the destination node using TCP sink.  
12. Configure window size and packet size for TCP.  
13. Set up FTP over the TCP connection.  
14. Configure UDP and TCP connections for source and destination.  
15. Create traffic generator (CBR) for source and destination.  
16. Define the plot window and finish procedure.  
17. In the finish procedure, declare global variables.  
18. Close the trace and name files, and execute the network animation file.  
19. At a specific time, call the finish procedure.  
20. Stop the program.
## PROGRAM:

#Lan simulation – mac.tcl setns [new Simulator] #define color for data flows
$ns color 1 blue
$ns color 2 red
set tracefile1 [openout.tr w]
$ns trace-all $tracefile1 #open nam file
set namfile [open out.namw]
$ns namtrace-all $namfile #define the finish procedure proc finish {}
{
global ns tracefile1 namfile
$ns flush-trace close $tracefile1 close $namfile
exec nam out.nam& exit 0
}
#create six nodes set n0 [$ns node] set n1[$ns node] set n2 [$ns node] set n3 [$ns node] set n4 [$ns node] set n5 [$ns node]
#Specify color and shape for nodes
$n1 color Red
$n1 shape box
#create links between the nodes
$ns duplex-link $n0 $n2 2Mb 10ms DropTail
$ns duplex-link $n1 $n2 2Mb 10ms DropTail
$ns simplex-link $n2 $n3 0.3Mb 100ms DropTail
$ns simplex-link $n3 $n2 0.3Mb 100ms DropTail # Create a LAN
set lan [$ns newLan "$n3 $n4 $n5" 0.5Mb 40ms LL Queue/DropTailMAC/Csma/Cd Channel] #Give node position
set lan [$ns newLan "$n3 $n4 $n5" 0.5Mb 40ms LL Queue/DropTailMAC/Csma/Cd Channel] #Give node position
$ns duplex-link-op $n0 $n2 orient right-down
$ns duplex-link-op $n1 $n2 orient right-up
$ns simplex-link-op $n2 $n3 orient right
 
$ns simplex-link-op $n3 $n2 orient left #setup TCP connection
set tcp [new Agent/TCP/Newreno]
$nsattach-agent $n0 $tcp
set sink [newAgent/TCPSink/DelAck]
$ns attach- agent $n4 $sink
$ns connect $tcp $sink
$tcp set fid_ 1
$tcp set packet_size_ 552 #set ftp over tcp connection set ftp [new Application/FTP]
$ftp attach-agent $tcp #setup a UDP connection set udp [new Agent/UDP]
$ns attach-agent $n1 $udp set null [new Agent/Null]
$ns attach-agent
$n5 $null
$ns connect $udp $null
$udp set fid_ 2
#setup a CBR over UDP connection setcbr [new Application/Traffic/CBR]
$cbr attach-agent $udp
$cbr set type_ CBR
$cbr set packet_size_ 1000
$cbr set rate_ 0.05Mb
$cbr set random_ false #scheduling the events
$ns at 0.0 "$n0 label TCP_Traffic"
$ns at 0.0 "$n1 label UDP_Traffic"
$ns at 0.3 "$cbr start"
$ns at 0.8 "$ftp start"
$nsat 7.0 "$ftp stop"
$ns at 7.5 "$cbr stop"
$ns at 8.0 "finish"
$ns run


## 📊 MODEL OUTPUT
<img src="C:\Users\Latha\OneDrive\Pictures\Screenshots\code block 2.png" alt="Code block 2" width="200"/>


## 📝 MARK ALLOCATION

| Criteria      | Total Marks | Marks Obtained |
|---------------|-------------|----------------|
| Performance   | 20          |                |
| Observation   | 20          |                |
| Record        | 20          |                |
| Output        | 20          |                |
| Viva          | 20          |                |
| **Total**     | **100**     |                |

## ✅ RESULT
Thus, the performance of the network with Carrier Sense Multiple Access/Collision Detection is verified using NS2 simulation.
