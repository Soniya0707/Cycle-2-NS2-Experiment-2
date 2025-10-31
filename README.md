# 🧪 Cycle-2-NS2-Experiment-2  
### PERFORMANCE ANALYSIS OF THE NETWORK WITH CSMA/CD

---

## 🎯 AIM
To write an **NS2 program** to observe the performance of a network using **Carrier Sense Multiple Access / Collision Detection (CSMA/CD)**.

---

## 🧰 EQUIPMENT REQUIRED
- PC System with Linux OS  
- NS2 Software Installed  

---

## 🧪 ALGORITHM
1. Start the program.  
2. Declare the global variable `ns` for creating a new simulator.  
3. Set the colors for packet flows.  
4. Open the **Network Animator (NAM)** file and **trace file** in write mode.  
5. Create required nodes for the network.  
6. Establish **duplex** and **simplex links** between nodes with delay, bandwidth, and queue mechanism.  
7. Create a **LAN** segment using **CSMA/CD** protocol.  
8. Assign positions for the nodes and links.  
9. Configure **TCP** and **UDP** connections for source and destination nodes.  
10. Set up **FTP** over TCP and **CBR** traffic over UDP.  
11. Define simulation events and finish procedure.  
12. Close the trace files, run NAM, and stop the simulation.  

---

## 💻 PROGRAM

```tcl
# Lan Simulation – mac.tcl

# Create a simulator object
set ns [new Simulator]

# Define colors for data flows
$ns color 1 blue
$ns color 2 red

# Open trace file
set tracefile1 [open out.tr w]
$ns trace-all $tracefile1

# Open NAM file
set namfile [open out.nam w]
$ns namtrace-all $namfile

# Define the finish procedure
proc finish {} {
    global ns tracefile1 namfile
    $ns flush-trace
    close $tracefile1
    close $namfile
    exec nam out.nam &
    exit 0
}

# Create six nodes
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node]
set n5 [$ns node]

# Specify color and shape for nodes
$n1 color red
$n1 shape box

# Create links between the nodes
$ns duplex-link $n0 $n2 2Mb 10ms DropTail
$ns duplex-link $n1 $n2 2Mb 10ms DropTail
$ns simplex-link $n2 $n3 0.3Mb 100ms DropTail
$ns simplex-link $n3 $n2 0.3Mb 100ms DropTail

# Create a LAN using CSMA/CD
set lan [$ns newLan "$n3 $n4 $n5" 0.5Mb 40ms LL Queue/DropTail MAC/Csma/Cd Channel]

# Give node positions
$ns duplex-link-op $n0 $n2 orient right-down
$ns duplex-link-op $n1 $n2 orient right-up
$ns simplex-link-op $n2 $n3 orient right
$ns simplex-link-op $n3 $n2 orient left

# Setup TCP connection
set tcp [new Agent/TCP/Newreno]
$ns attach-agent $n0 $tcp

set sink [new Agent/TCPSink/DelAck]
$ns attach-agent $n4 $sink

$ns connect $tcp $sink
$tcp set fid_ 1
$tcp set packet_size_ 552

# Set FTP over TCP
set ftp [new Application/FTP]
$ftp attach-agent $tcp

# Setup UDP connection
set udp [new Agent/UDP]
$ns attach-agent $n1 $udp

set null [new Agent/Null]
$ns attach-agent $n5 $null

$ns connect $udp $null
$udp set fid_ 2

# Setup CBR over UDP
set cbr [new Application/Traffic/CBR]
$cbr attach-agent $udp
$cbr set type_ CBR
$cbr set packet_size_ 1000
$cbr set rate_ 0.05Mb
$cbr set random_ false

# Scheduling the events
$ns at 0.0 "$n0 label TCP_Traffic"
$ns at 0.0 "$n1 label UDP_Traffic"
$ns at 0.3 "$cbr start"
$ns at 0.8 "$ftp start"
$ns at 7.0 "$ftp stop"
$ns at 7.5 "$cbr stop"
$ns at 8.0 "finish"

# Run the simulation
$ns run
```


## 📊 MODEL OUTPUT
![WhatsApp Image 2025-10-24 at 09 06 09_9b001374](https://github.com/user-attachments/assets/695ce8d9-3aa8-4056-8659-cac108e68434)



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
