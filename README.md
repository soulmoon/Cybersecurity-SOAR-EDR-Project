# 🛡️ Cybersecurity SOAR-EDR Project

A complete hands-on Security Orchestration, Automation, and Response (SOAR) + Endpoint Detection and Response (EDR) lab using **Tines**, **LimaCharlie**, **Slack**, and **Email** to detect, alert, and respond to malicious activity — specifically detecting the use of the **LaZagne** credential dumping tool.

## 🔧 Technologies Used

- **SOAR**: [Tines](https://tines.com)
- **EDR**: [LimaCharlie](https://limacharlie.io)
- **Communication**: Slack, Email

## 🎯 Project Objectives

- ✅ Create custom Detection & Response rule in LimaCharlie
- ✅ Design a SOAR playbook in Tines
- ✅ Integrate Slack & Email for alerting
- ✅ Automate host isolation based on user input

### Part 1: SOAR-EDR Playbook Design

- **Detection Trigger**: LimaCharlie detects a hacktool (LaZagne)
- **Workflow**:
  1. Tines receives detection via webhook
  2. Sends alert to Slack & Email with:
     - Time
     - Computer Name
     - Source IP
     - Command Line
     - File Path
     - Sensor ID
     - Detection Link
  3. User prompt in Tines: “Do you want to isolate the machine?”
     - **Yes** → Isolate the endpoint via LimaCharlie
     - **No** → Notify for further investigation

<img width="940" height="759" alt="image" src="https://github.com/user-attachments/assets/f7b53ae9-dc16-418e-83b4-ce7d4d07632f" />

### Part 2: Setup LimaCharlie

1. Create an account and installation key
2. Download and Install agent on a Windows Server using:
   Command:
   lc_sensor.exe -I YOUR_INSTALLATION_KEY
   >> YOUR_INSTALLATION_KEY is the Session Key which you can find in LimaCharlie Portal
4. Confirm LimaCharlie Service is running in Win Server
5. Confirm the Win Server is visible under LimaCharlie Portal Sensor List
<img width="940" height="394" alt="image" src="https://github.com/user-attachments/assets/c56cc4a2-9f85-4216-a07d-4bc823854cc8" />
6. From LimaCharlie Portal, run remote actions on WIN Server
<img width="940" height="464" alt="image" src="https://github.com/user-attachments/assets/e083eb8f-ac9b-493c-84d8-0adf21023958" />

### Part 3: Generate Telemetry for Lazgne and Create D&R rules in LimaCharlie

1. Install Lazagne in Win Server
2. Download from GitHub
3. Run LaZagne and observe the process via LimaCharlie
4. Create D&R Rule in LimaCharlie Portal
5. Goto Automation
6. D&R Rules
7. New Rule (Response + Detect)
8. Response Rule:
        events:
        - NEW_PROCESS
        - EXISTING_PROCESS
        op: and
        rules:
        - op: is windows
        - op: or
          rules:
          - case sensitive: false
            op: ends with
            path: event/FILE_PATH
            value: lazagne.exe
          - case sensitive: false
            op: ends with
            path: event/COMMAND_LINE
            value: all
          - case sensitive: false
            op: contains
            path: event/COMMAND_LINE
            value: lazagne
          - case sensitive: false
            op: is
            path: event/HASH
            value: 'dc06d62ee95062e714f2566c95b8edaabfd387023b1bf98a09078b84007d5268'
9. Detect Rule:
       - action: report
          metadata:
            author: dipak
            description: Detects Lazagne (SOAR-EDR Tool)
            falsepositives:
            - Unlikely
            level: medium
            tags:
            - attack.credential_access
          name: dipak - HACKTOOL - Lazagne (SOAR-EDR)
10. Test Rule by clicking on Test Event
<img width="940" height="466" alt="image" src="https://github.com/user-attachments/assets/2f02384a-4b25-4cf2-84f1-296873a5ad07" />

12. Confirm logs under Detection
<img width="940" height="536" alt="image" src="https://github.com/user-attachments/assets/a1dc25de-d692-4447-92c7-1c6b5a2d71a7" />

### Part 4: Setup and Integrate Slack & Tines

1. Create Slack channels (e.g., alerts)
2. Create an account in Tines
3. In Tines:
  4. Drag in Webhook → Retrieve detections from LimaCharlie
  5. Configure LimaCharlie output → Paste webhook URL in Detections > Output > Tines
  6. Re-run LaZagne to generate sample events
  7. Validate events are received in Tines
<img width="940" height="674" alt="image" src="https://github.com/user-attachments/assets/51746d64-f4b5-4fd8-8fa2-336b98429641" />

<img width="940" height="534" alt="image" src="https://github.com/user-attachments/assets/dcd77958-06e8-4f33-b33c-32ba0845e197" />

### Part 5: Alerting + Automated Response

1. Connect Tines with Slack:
2. Add Slack credentials and channel ID
3. Send messages with detection info
<img width="940" height="535" alt="image" src="https://github.com/user-attachments/assets/1eca9e69-e1a5-4ba3-8929-5e9e0cc6bafd" />

4. Connect Tines with Email:
5. Configure a test email address
6. Receive structured detection alerts
<img width="880" height="476" alt="image" src="https://github.com/user-attachments/assets/4c712296-e67e-426f-95d6-2112c2f84988" />

7. Add User Prompt:
8. Select Page in Tines and Edit Page with Heading: Dipak SOC Lab SOAR-EDR
9. Content: Do you want to isolate the Machine?
10. Add Boolean for Yes or No
11. Add Button for Submit
<img width="940" height="439" alt="image" src="https://github.com/user-attachments/assets/fc6142b7-2044-4af9-b474-97cd15f918e7" />

🧪 Final Test Results
✅ Slack alerts received with full detection context

✅ Email alerts delivered

✅ User Prompt in Tines functional

✅ LimaCharlie successfully isolated the machine when prompted

✅ End-to-end Playbook execution verified


🧠 Takeaways
Developed an end-to-end SOAR-EDR workflow from scratch

Built custom detection and automated response logic

Integrated multiple platforms to simulate a real-world SOC environment

✍️ Author
Dipak Pakhrin






