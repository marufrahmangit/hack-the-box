# Scenario
The IDS device alerted us to a possible rogue device in the internal Active Directory network. The Intrusion Detection System also indicated signs of LLMNR traffic, which is unusual. It is suspected that an LLMNR poisoning attack occurred. The LLMNR traffic was directed towards Forela-WKstn002, which has the IP address 172.17.79.136. A limited packet capture from the surrounding time is provided to you, our Network Forensics expert. Since this occurred in the Active Directory VLAN, it is suggested that we perform network threat hunting with the Active Directory attack vector in mind, specifically focusing on LLMNR poisoning.

# Goal
- Investigate through network traffic and uncover credential-stealing technique by abusing the LLMNR protocol feature in Windows. 
- Learn how a victim made a typo navigating to a network share and how the attacker was using the Responder tool to steal hashes and pose as a legitimate device in the internal network. 
- Learn to crack NTLMV2 hashes by gathering information from SMB traffic.

# Understanding LLMNR poisoning attack
### What is LLMNR?
LLMNR is a protocol that allows both IPv4 and IPv6 hosts to perform name resolution for hosts on the same local network without requiring a DNS server or DNS configuration.

When a host’s DNS query fails (i.e., the DNS server doesn’t know the name), the host broadcasts an LLMNR request on the local network to see if any other host can answer.

LLMNR is the successor to NetBIOS.  NetBIOS (Network Basic Input/Output System) is an older protocol that was heavily used in early versions of Windows networking. NBT-NS is a component of NetBIOS over TCP/IP (NBT) and is responsible for name registration and resolution.  Like LLMNR, NBT-NS is a fallback protocol when DNS resolution fails. It allows local name resolution within a LAN.

### How is LLMNR vulnerable?
LLMNR has no authentication mechanism.  Anyone can respond to an LLMNR request, which opens the door to potential attacks.  When a computer tries to resolve a domain name and fails via the standard methods (like DNS), it sends an LLMNR query across the local network.  An attacker can listen for these queries and respond to them, leading to potential unauthorized access.

![image](https://github.com/user-attachments/assets/44e94531-e945-407f-bc19-77ae3569c0ba)

# Response
To find the malicious IP address, first, I identified the IP Address of the domain controller. Any LLMNR requests originating from some other machine to another machine is a sign of a rogue machine pretending to be a Domain controller to capture hashes. I filtered for *UDP port 5355* in wireshark using `"udp.port == 5355"`. This will display all the LLMNR traffic. I found only 1 IP address responding to the victim machine to their query. This IP Address does not belong to the domain controller.

![image](https://github.com/user-attachments/assets/cb76d69f-1342-4325-9594-3d8a6a5cfe97)

To find the rougue machine's hostname, I added a filter for the IP address and DHCP in Wireshark using `"ip.addr == 172.17.79.135 && dhcp"`. Then I looked for the hostname value in one of the packets. The reason why I used this is because the device used dhcp to get an ip address assigned to itself and map it to its hostname.

![image](https://github.com/user-attachments/assets/67ed17fe-da79-4151-9cb2-0bdf664c5c47)

Next, I needed to confirm whether the attacker captured the user's hash using the SMB traffic filter in Wireshark using `"smb2"`. Here I found some `ntlmssp negotiate and auth` which means the hash indeed got captured.

![image](https://github.com/user-attachments/assets/73e207f2-bf3c-45f6-81ca-667c21cf1e7c)

To further narrow down this, I added the `"ntlmssp"` filter and found the username in `NTLMSSP_AUTH` packets.

![image](https://github.com/user-attachments/assets/2ab80ae8-9a01-4be0-ae6a-aed1a9a35751)





