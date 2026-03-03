<h1>Azure Sentinel SOC</h1>

<h2>Objective</h2>
Build a cloud-based SOC to monitor live cyberattacks by exposing a vulnerable VM to the public internet. The goal was to use Microsoft Sentinel to capture, analyze, and map global brute-force attempts in real-time.
<br />

<h2>Project Summary</h2>
In this lab, I deployed a "Honeypot" VM with disabled firewalls and open RDP ports to attract global attackers. Using the Azure Monitor Agent and KQL, I ingested failed login data into Microsoft Sentinel, paired it with geographic data, and visualized the attack origins on an interactive world map.
<br />

<h2>Languages and Utilities Used</h2>

- <b>KQL</b> 
- <b>PowerShell</b>
- <b>RDP</b>

<h2>Environments Used </h2>

- <b>Microsoft Azure</b>
- <b>Microsoft Sentinel</b>
- <b>Windows 10</br>
<br>

<h2>Project Report:</h2>
<br>


<p align="left">
<h2>Section 1: Infrastructure Deployment</h2>
<p align="left">
<h2>Step 1: Creating the Resource Group</h2>
 
- <b>Purpose: To provide a logical container for all lab resources, ensuring they can be managed from a single point.</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/e50fe811-c15f-4003-be9a-0134860902dc" />

<h2>Step 2: Creating a Virtual Network</h2>
 
- <b>To create an isolated private network in the cloud where the VM can communicate with other Azure services.</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/90cd2232-c8a8-45f5-a5e5-ca0ca243d5ec" />


<h2>Step 3: Creating the Virtual Machine / Honeypot</h2>
 
- <b>To act as the vulnerable target exposed to the internet to attract and record unauthorized access attempts.</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/7810b4cc-85ff-40e7-9bcf-bb4155810dba" />



- <b>View of Resource Group with resources created so far:</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/68684fe4-0595-4cee-a5e8-669e44d2067a" />

<h2>Section 2: The Honeypot Setup</h2>
<h2>Step 1: Delete/Modify Network Security Group (NSG) Rules:</h2>
 
- <b>To bypass Azure’s default security rules, allowing all inbound traffic (Port 3389 and ICMP) from any source IP on the internet.</b>
<p align="center">
<img width="500" height="150" alt="image" src="https://github.com/user-attachments/assets/98051c35-8dda-4328-a590-60307091346c" />
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/19933317-27d0-479c-a7c7-0a638fd29211" />

<h2>Step 2: RDP and Disable Guest Firewalls</h2>
 
- <b>To remove the final layer of defense within the Windows VM, ensuring that network traffic isn't blocked by the machine.</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/2344326a-1215-4713-9658-386bff068ce3" />

<h2>Step 3: Ping VM from Host:</h2>
 
- <b>To verify the honeypot is live and reachable from the public internet before beginning log collection.</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/db5c94cf-3c11-4bfa-a71b-e6abf1290e8a" />

<h2>Section 3: Logging and Event Generation</h2>
<h2>Step 1: Generate Incorrect Logins</h2>
 
- <b>To manually create "Audit Failure" events (Event ID 4625) in the Windows Event Viewer, providing data to test the logging. </b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/23d474a7-4ed4-4d09-8ffe-0ae83762ea8c" />

<h2>Step 2: View Events in Event Viewer</h2>
 
- <b>To confirm that the VM is successfully capturing the security logs locally before attempting to ship them to the cloud.</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/29cce382-b1f2-44ee-b428-a1ce462f5c53" />
  
- <b>Failed Login Event via Event Viewer:</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/705a23ba-658e-46b3-9cda-cc0482d81b6e" />

<h2>Section 4: SIEM & Data logging through Microsoft Sentinel</h2>
<h2>Step 1: Deploy Log Analytics Workspace (LAW) and Add Sentinel:</h2>
 
- <b>To create the centralized database where all raw security logs will be stored and queried using KQL.</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/24501a32-60ef-4d82-b498-f6ea36348988" />

<h2>Step 2: Install Windows Security Events / via AMA:</h2>
 
- <b>To enable the specific Microsoft Sentinel bridge that allows the workspace to recognize and receive streamed security events.</b>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/4df9bc96-7413-44ba-b52e-9b80a07240c9" />

<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/2af790da-e8f4-43ff-9802-03f5917e8a27" />

<h2>Section 5: Data Visualization</h2>
<h2>Step 1: Create Watchlist (IP Geo-location)</h2>
 
- <b>To pair raw IP addresses with external geographic data, allowing the SIEM to identify the physical origin of attackers.</b>
<p align="center">
 <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/2ce1d394-3d65-478c-8a28-cdc707d5c7b1" />

<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/0bbec7d0-feb3-4c98-af4b-1111e5a83342" />

<h2>Step 2: Generate the Visual Map:</h2>
 
- <b>To transform KQL query results into a visual "Workbook" or Dashboard, providing a geographic map of global attacks.</b>

<p align="center">
<img width="975" height="347" alt="image" src="https://github.com/user-attachments/assets/6fe48fe1-9763-4851-a409-cc45787167bb" />
<p align="center">
<img width="975" height="500" alt="image" src="https://github.com/user-attachments/assets/8196f5b8-ea5b-4046-a15c-3e4ca1de011a" />

<h2>After 12 Hours: </h2>
<p align="center">
<img width="975" height="280" alt="image" src="https://github.com/user-attachments/assets/2de40932-6f50-416c-88ef-8f2bd3d16939" />
<p align="center">
<img width="975" height="278" alt="image" src="https://github.com/user-attachments/assets/f993ce49-950f-44f7-ae02-075e427ab5a6" />

<h2>After 24 Hours: </h2>
<p align="center">
<img width="975" height="278" alt="image" src="https://github.com/user-attachments/assets/048e2a72-9a22-468c-bd13-a766ec28520c" />
<p align="center">
<img width="975" height="276" alt="image" src="https://github.com/user-attachments/assets/8cafc1b2-605d-4724-9a0e-9cc82fa92714" />


<h2>Final Summary</h2>
The objective of this lab was to establish a cloud-based Security Operations Center (SOC) environment to observe, log, and visualize real-world cyberattacks. By intentionally deploying a vulnerable Windows Virtual Machine (Honeypot) and exposing it to the public internet, I successfully attracted automated brute-force activity. Utilizing Microsoft Sentinel, I collected security event data, combined it with geographic data via custom Watchlists, and produced a visual Attack Map. This project demonstrates a complete security workflow: from infrastructure hardening (and intentional weakening) to advanced threat visualization.
<br />

<h2>Key Takeaways:</h2>

- <b>Rapid Discovery:</b> Once the Network Security Group rules were modified to allow all traffic, the virtual machine was discovered by automated internet scanners within hours, highlighting the speed at which exposed resources are targeted and the dangers of lacking security or accidental vulnerability creation.
- <b>Global Threat Actor Origin: </b> Through the use of KQL and Geo-IP Watchlists, I observed that attack attempts originated from diverse geographic regions, with significant clusters appearing in the Netherlands, United States, and Southeast Asia.
- <b>SIEM Benefits: </b> The lab proved that raw logs are difficult to interpret in isolation, but by using Workbooks in Sentinel, I was able to transform thousands of security alerts into a heat map for data visualization.
