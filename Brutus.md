# Scenario
You will familiarize yourself with Unix auth.log and wtmp logs. We'll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using auth.log. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.

# Goal
The goal is to analyze the auth.log and the wtmp log files and answer the following questions as part of the incident response:

### Analyzing the auth.log, can you identify the IP address used by the attacker to carry out a brute force attack?
It appears that the IP 65.2.161.68 continuously keeps failing to authenticate. These logs show someone trying to log in as admin, and the system saying that there is no user admin. These failed login run from 06:31:33 to 06:31:42, suggesting a brute force tool or script is running, as a user at the keyboard could not type that fast:
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/9de73f1f-2085-4d7c-8c9b-5e7dce21dc89)

### The brute force attempts were successful, and the attacker gained access to an account on the server. What is the username of this account?
root:
