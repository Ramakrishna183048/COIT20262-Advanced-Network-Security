# week 1

## Task 2 – TCP SYN Flood DoS Attack

### Objective
The objective of this task was to demonstrate and observe a TCP SYN flood attack in a controlled virtual lab environment using Kali Linux and Metasploitable 2.

### Lab Setup
- Kali Linux: `172.16.1.34`
- Metasploitable 2: `172.16.1.35`
- Target service: HTTP (TCP port 80)
- Network: VirtualBox Internal Network

### Network Connectivity
Kali Linux and Metasploitable 2 were configured on the same internal network. Connectivity between the two virtual machines was verified successfully before conducting the experiment.

![Network Connectivity](images/Week2_Task2_Connectivity.png)

### TCP SYN Flood and Packet Detection
A TCP SYN flood was generated from Kali Linux towards the HTTP service running on Metasploitable 2. During the experiment, `tcpdump` was used on Kali to monitor the traffic on the lab network interface.

The packet capture showed a large number of TCP SYN packets (`Flags [S]`) directed towards the HTTP service. The test was stopped after the required short period using `Ctrl+C`.

![TCP SYN Flood](images/Week2_Task2_SYN_Flood_TCPdump.png)

### Observing the Effect on Metasploitable 2
The web service on Metasploitable 2 was monitored using the `ss` command. Before the second test, the HTTP service was shown in the `LISTEN` state.

A second SYN flood test containing additional packet data was then performed from Kali Linux.

![SYN Flood Test](images/Week2_Task2_SYN_Flood_Effect.png)

After the test, the HTTP service was checked again. In my experiment, the service remained in the `LISTEN` state.

![Metasploitable Result](images/Week2_Task2_ms2_After_SYN_Flood.png)

### Conclusion
This task demonstrated how a TCP SYN flood can generate a very large number of SYN packets towards a network service within a short period. Using `tcpdump` made it possible to observe the SYN traffic, while `ss` was used to monitor the HTTP service on the target machine. The experiment was performed only within the controlled VirtualBox lab environment.
