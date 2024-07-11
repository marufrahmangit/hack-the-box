# Scenario
Forela's Network is constantly under attack. The security system raised an alert about an old admin account requesting a ticket from KDC on a domain controller. Inventory shows that this user account is not used as of now so you are tasked to take a look at this. This may be an AsREP roasting attack as anyone can request any user's ticket which has preauthentication disabled.
Log files: [campfire-2.zip](https://github.com/user-attachments/files/16158289/campfire-2.zip) pass: hacktheblue

# Goal
The goal is to investigate Windows event logs to detect common Active Directory credential attacks such as ASREP Roasting and Kerebroasting by focusing on key event IDs and key attributes.

# Response
### When did the ASREP Roasting attack occur, and when did the attacker request the Kerberos ticket for the vulnerable user?
Check the Event ID log reference (found online):
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/d1994b15-a22b-404d-a762-f4184c2bc85a)

Use the event ID 4768 to filter the log to find Kerberos authentication ticket requests:
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/b588dfbf-700d-46ba-b66e-e980162a1344)

Among the events, the one with __pre-authentication type set to 0__ (General tab) satisfies this criteria:
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/21826105-3173-4c07-9ab4-fdb75d6f8f4b)

Check the XML view (Details tab) to see the timestamp. The answer is ***5/29/2024 1:36:40 PM***:
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/9fdc6063-b3ae-466e-a965-7dfa4394860a)

### Please confirm the User Account that was targeted by the attacker.

***arthur.kyle***
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/7c1dac70-b01c-4aaa-8464-257e111a29b4)

### What was the SID of the account?

***S-1-5-21-3239415629-1862073780-2394361899-1601***
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/14e8241e-6c61-4490-9222-6c2ffe8664f5)

### It is crucial to identify the compromised user account and the workstation responsible for this attack. Please list the internal IP address of the compromised asset to assist our threat-hunting team.

***172.17.79.129***
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/a2bf9826-468c-4414-8618-2e3bfd1ae014)

### We do not have any artifacts from the source machine yet. Using the same DC Security logs, can you confirm the user account used to perform the ASREP Roasting attack so we can contain the compromised account/s?

***happy.grunwald***
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/17e9de75-dce4-4a9c-b007-1af04e3cf244)


