<h1 align="center">Network Traffic Analysis: Redline Stealer Malware Investigation</h1>

<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; border-left: 5px solid #ff4d4d; color: #ffffff;">
<strong>Tools & Resources Used:</strong>
<ul>
<li><a href="https://www.wireshark.org/" style="color: #4db8ff;">Wireshark (Network Protocol Analyzer)</a></li>
<li><a href="https://www.malware-traffic-analysis.net/" style="color: #4db8ff;">Malware-Traffic-Analysis.net (Dataset Source)</a></li>
<li><a href="https://github.com/KaucherIslam/Network-Intrusion-Forensics-Redline-Stealer-Traffic-Analysis/blob/main/Redline-Stealer.pcap" style="color: #4db8ff;">2024-10-23_Redline_Stealer.pcap (The Packet Capture File Used)</a></li>
</ul>
</div>

<h2>1. Initial Discovery and C2 Communication</h2>

<p>The investigation began by isolating traffic associated with the Redline Stealer infection. I identified the Command and Control (C2) server operating on a non-standard port and using a specific web service for reconnaissance.</p>

<ul>
<li><b>Infrastructure Identification:</b> The malware communicated with <b>188.190.10.10</b> over port <b>55123</b>.</li>
<li><b>External Reconnaissance:</b> I observed DNS queries for <code>api.ip.sb</code>, a tactic used by the malware to determine the victim's public IP address before proceeding with data theft.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-03-26 211752" src="https://github.com/user-attachments/assets/be8efb08-f235-42e2-b203-cef706fe7b51" />
<br />
<em>Figure 1: Identifying the initial C2 handshake and DNS reconnaissance queries.</em>
</p>

<h2>2. Victim Profiling and System Fingerprinting</h2>

<p>Redline Stealer is highly efficient at profiling its victims. By analyzing the outbound HTTP POST requests, I intercepted the raw XML data containing the compromised machine's identity.</p>

<ul>
<li><b>The Data:</b> Using the <code>SetEnvironment</code> SOAP action, the malware exfiltrated the hostname, Windows version, and hardware specs in plain text.</li>
<li><b>Forensic Highlight:</b> I manually highlighted the <b>&lt;a:MachineName&gt;user1&lt;/a:MachineName&gt;</b> tag to demonstrate the exact point where the victim's identity was stolen.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-03-26 212238" src="https://github.com/user-attachments/assets/31d6254d-4647-416a-8424-cf6b65fc3c09" />
<br />
<em>Figure 2: Victim system profile extracted from a malicious SOAP/XML payload.</em>
</p>

<h2>3. Malware Persistence and Evasion</h2>

<p>The analysis revealed where the malware was hiding within the system architecture. This is a critical step in understanding the infection's "footprint" on the host.</p>

<ul>
<li><b>Execution Path:</b> The traffic confirmed the malware was running out of <code>C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe</code>.</li>
<li><b>Significance:</b> Highlighting this specific path shows the malware’s attempt to blend in with legitimate Windows .NET processes to avoid detection by basic task management tools.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-03-26 212358" src="https://github.com/user-attachments/assets/c6189d2e-2400-4dfb-89c3-163a6c880dba" />
<br />
<em>Figure 3: Identifying the malicious process path used for evasion and persistence.</em>
</p>

<h2>4. Targeted Assets and Stealer Instructions</h2>

<p>The final phase of the analysis focused on the "EnvironmentSettings" instructions sent by the C2 server. This revealed the specific high-value targets the attacker was interested in.</p>

<ul>
<li><b>Browser Targeting:</b> The C2 server sent a massive list of file paths to the infected host. I highlighted the segments where the malware was commanded to search for <b>Google Chrome</b> and <b>Mozilla Firefox</b> user data.</li>
<li><b>The Goal:</b> This confirms the intent to harvest saved passwords, browser cookies, and autofill credit card information.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-03-26 212638" src="https://github.com/user-attachments/assets/23eb7837-d873-4993-84e9-06cf149aa74d" />
<br />
<em>Figure 4: Intercepting the attacker's instructions for targeting browser-stored credentials.</em>
</p>

<h2>Technical Skills Demonstrated</h2>

<ul>
<li><b>Network Forensics:</b> Navigating complex PCAP files to isolate malicious streams from background noise.</li>
<li><b>Protocol Analysis:</b> Decoding SOAP/XML traffic over HTTP to read unencrypted C2 commands.</li>
<li><b>Threat Intelligence:</b> Identifying Indicators of Compromise (IOCs) such as C2 IPs and malicious file paths.</li>
<li><b>Visual Reporting:</b> Enhancing technical findings with manual highlights to ensure rapid data comprehension for stakeholders.</li>
</ul>

<h1>Final Reflection</h1>

<p>This project demonstrates the vulnerability of unencrypted application-layer protocols. By analyzing the Redline Stealer PCAP, I successfully reconstructed the infection lifecycle—from initial reconnaissance to the targeting of specific browser assets. The most significant takeaway was the importance of deep packet inspection; by manually highlighting the XML payloads, I was able to transform raw network data into a clear story of how an infostealer operates. This lab proves my ability to identify not just that a system is infected, but exactly what data is at risk.</p>

<p>Thank you for taking the time to review my project and for your consideration of my work.</p>
