# Scenario
Forela's Network is constantly under attack. The security system raised an alert about an old admin account requesting a ticket from KDC on a domain controller. Inventory shows that this user account is not used as of now so you are tasked to take a look at this. This may be an AsREP roasting attack as anyone can request any user's ticket which has preauthentication disabled.
Log files: [campfire-2.zip](https://github.com/user-attachments/files/16158289/campfire-2.zip) pass: hacktheblue

# Goal
The goal is to analyze the event log file and answer the following questions as part of the incident response.

### When did the ASREP Roasting attack occur, and when did the attacker request the Kerberos ticket for the vulnerable user?
