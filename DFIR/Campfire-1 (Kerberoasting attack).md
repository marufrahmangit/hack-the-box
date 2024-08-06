# Scenario
Alonzo Spotted Weird files on his computer and informed the newly assembled SOC Team. Assessing the situation it is believed a Kerberoasting attack may have occurred in the network. It is your job to confirm the findings by analyzing the provided evidence. You are provided with: 1- Security Logs from the Domain Controller 2- PowerShell-Operational Logs from the affected workstation 3- Prefetch Files from the affected workstation.

# Goal
Confirm the findings by analyzing the provided evidence: the logs and the prefetch files, and answer the following questions as part of the incident response. Log files:[campfire-1.zip](https://github.com/user-attachments/files/16174057/campfire-1.zip) pass: hacktheblue

# Response
### Analyzing Domain Controller Security Logs, can you confirm the date & time when the kerberoasting activity occurred?
In Security Logs, Filter for Event ID 4769. Now Look for any event where the service name is NOT( krbtgt or ends with $ (For e.g DC01$ ) ). The ticket type should be 0x17 which is for RC4 type encryption. The failure code should be 0x0. The event that matches all the above conditions is the event detailing information about the kerberoasting attack activity.

***2024-05-21 03:18:09***
![image](https://github.com/marufrahmangit/hack-the-box/assets/25085219/9d957a19-7dd7-4b7b-939a-99b7c4e2e9c5)

### What is the Service Name that was targeted?

***MSSQLService***

### It is really important to identify the Workstation from which this activity occurred. What is the IP Address of the workstation?

***172.17.79.129***

Now that we have identified the workstation, a triage including PowerShell logs and Prefetch files are provided to you for some deeper insights so we can understand how this activity occurred on the endpoint. What is the name of the file used to Enumerate Active directory objects and possibly find Kerberoastable accounts in the network?

Using powershell logs and filter for event ID 4104: ***powerview.ps1***

### When was this script executed?

***2024-05-21 03:16:32***

### What is the full path of the tool used to perform the actual kerberoasting attack?
Finding the answer required a few steps:
1. Download the PeCMD (zimmerman tool) and add it to the environment variable (Windows OS):
![image](https://github.com/user-attachments/assets/1baa22e0-2e6a-4505-9e72-ecff1bd86ac4)
2. Parse the prefetch files using the command: **Pecmd.exe -d "Path of prefetch Artifacts" --csv . --csvf result.csv**
3. Open the parsed ***result.csv*** file using TimelineExplorer (zimmerman tool)
4. Find the timeline established so far; a row gets filters using this timeline > ***2024-05-21 03:18:09***
5. In the "Files Loaded" column copy the data from the cell and arrange them vertically for easier readability (optional step/ used chatgpt to do this)
6. Of all the file names this looked suspicious and it is also an executable file: 
![image](https://github.com/user-attachments/assets/7df9a7ef-2b11-4724-b57b-f52a05cae120)
***\VOLUME{01d951602330db46-52233816}\USERS\ALONZO.SPIRE\DOWNLOADS\RUBEUS.EXE***

The answer is ***C:\Users\Alonzo.spire\Downloads\Rubeus.exe***

### When was the tool executed to dump credentials?
According to the  "Last Run" Value in the PEcmd output the answer is ***2024-05-21 03:18:08***
![image](https://github.com/user-attachments/assets/3501243a-cea5-42b4-8a50-6e1dc6ea8e8f)


