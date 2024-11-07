# Man-in-the-Middle (MITM) Attack Using ARP Spoofing

## Overview
This project documents a man-in-the-middle (MITM) attack using ARP spoofing, where the goal was to intercept communications between two parties, Alice and Bob, without their knowledge. The task was completed as part of a practical exercise where I assumed the role of Mallory, the attacker.

## Objectives
- **Establish a Man-in-the-Middle Position**: Perform ARP spoofing to impersonate one of the machines in the network and intercept traffic.
- **Extract a Secret**: Capture and analyze network traffic to obtain a secret that Alice shares with Bob.
- **Bonus**: Explore methods to maintain a MITM position even when Alice and Bob use intrusion detection systems (IDS) that detect modified ARP replies.

## Approach
### Setting Up the Environment
1. **Access Mallory's PC**: Logged into Mallory's machine using the provided credentials and configured the network interface (`eth10`).
    - Command used: `ifconfig eth10`
    - MAC Address of Mallory: `c0:00:00:00:00:10`

2. **Network Traffic Analysis**:
    - Used `tcpdump` to analyze the network traffic on interface `eth10`.
    - Captured repeated ARP requests from Alice asking for Bob's MAC address.
    - Command used: `sudo tcpdump -i eth10 -Xn`

3. **ARP Table Inspection**:
    - Checked the ARP table on Mallory's PC to identify the MAC addresses of Alice and Bob.
    - Command used: `arp -i eth10`

### ARP Spoofing and MITM Attack
1. **Sending ARP Replies**:
    - Used the provided `RAW_Packet.c` program to craft and send ARP replies.
    - Sent an ARP reply to Alice, impersonating Bob by using Bob's IP address and Mallory's MAC address.
    - Command used: `sudo raw_packet eth10 <Alice's MAC Address> 0x0806 <payload_file_name>`

2. **Intercepting and Analyzing Traffic**:
    - Continued to monitor the network traffic using `tcpdump`.
    - Captured the packet where Alice shared a secret with Bob.
    - Observed that the packet was forwarded to Bob, allowing Mallory to intercept the communication.

### Bonus: Handling Intrusion Detection Systems
- **Assumption**: Alice and Bob use an IDS that detects modified ARP replies.
- **Strategy**:
    - Sent an ARP reply to Alice using Bob's IP address but Mallory's MAC address to ensure Mallory's MAC was stored in Alice's ARP cache.
    - Created a fake ARP reply hex file (payload) to maintain the MITM position even under IDS monitoring.

## Tools and Commands Used
- **`tcpdump`**: Used for capturing and analyzing network traffic.
- **`arp`**: Used to inspect and manipulate ARP tables.
- **`raw_packet`**: A custom C program used for crafting and sending raw Ethernet packets.
- **`ifconfig`**: Used to configure network interfaces.

## Results
- **Secret Obtained**: Successfully intercepted the secret shared by Alice with Bob.
- **Bonus Part**: Explored methods to maintain a MITM position despite the presence of an IDS.

## Conclusion
The task successfully demonstrated how ARP spoofing can be used to intercept and analyze communications in a switched Ethernet network. It also highlighted potential countermeasures, such as IDS, and the importance of securing ARP communications.

## Acknowledgments
This project was completed as part of a cybersecurity exercise in an educational setting.
