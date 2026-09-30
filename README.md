# My Cybersecurity Engineering Portfolio

This repository hosts a personal project I built to understand enterprise visibility, log ingestion, and detection engineering. I wanted to move past basic theory and build a lab that mimics how real Security Operations (SecOps) teams monitor networks and catch malicious activity.

---

## Enterprise EDR Telemetry & Detection Engineering Lab

### Project Breakdown
In a corporate banking environment, security teams need deep visibility to catch attackers before they can move laterally through a network. This lab focuses on setting up a real-time event logging pipeline, analyzing endpoint telemetry data, and writing custom detection rules from scratch.

* **Target System:** Sandboxed Windows 11 Virtual Machine (Hosted via Oracle VirtualBox)
* **EDR Agent:** Kernel-level cloud-native Endpoint Detection & Response (EDR) Agent via LimaCharlie
* **Log Analytics Engine:** Cloud Event Ingestion Platform & Data Pipeline
* **Framework Correlation:** MITRE ATT&CK Framework Tactics and Techniques

---

### How I Built It

#### 1. Deploying the EDR Pipeline
I started by setting up a cloud security organization tenant named `BofA-SecOps-Enclave`. To get telemetry streaming from my Windows 11 VM, I generated an installation key and deployed a lightweight sensor agent using an administrative PowerShell terminal. This established a live, continuous feed tracking processes, file mutations, and network sockets from the operating system directly to my cloud console.

#### 2. Running an Attacker Simulation
To test the environment's visibility, I simulated a common post-exploitation technique used during adversary reconnaissance phases. Inside the Windows terminal, I ran:
```powershell
whoami /priv
```
Attackers frequently run this discovery command immediately after gaining access to check their privilege levels and map out potential paths for privilege escalation.

#### 3. Log Ingestion & Custom Rule Design
Once the command was executed, I jumped over to the live telemetry stream to inspect the raw log data. I isolated the process creation event and analyzed its raw JSON configuration string, picking out the process path (`C:\WINDOWS\system32\whoami.exe`) and the explicit command line arguments used.

Using this information, I built a custom Detection and Response (D&R) rule using specific string matching to flag unauthorized system discovery behaviors without disrupting standard user behavior.

#### 4. Testing & Catching the Exploit
After deploying the new rule to the cloud engine, I ran the simulation command inside the Windows VM again. The detection engine successfully intercepted the process creation event, matched my custom logic parameters, and instantly fired a live alert (`BofA_Simulated_Privilege_Abuse_Alert`) straight to my security dashboard, complete with the full JSON metadata for incident analysis.

---

### Practical Validation Evidence

#### Phase 1: Running the Attacker Simulation
*Verifying endpoint execution. This screenshot shows the local terminal processing the privilege audit command.*

![Attacker Simulation Check](IMG1.JPG)

#### Phase 2: Building the Detection Rule
*Configuring the logic. This screenshot shows my custom YAML/JSON rule structure saved successfully into the detection console.*

![Detection Signature Commitment](IMG2.JPG)

#### Phase 3: The Live Alert Capture
*Validating the defense loop. The main dashboard catches the activity and logs the high-fidelity alert live.*

![Production Alert Mitigation](IMG3.JPG)
