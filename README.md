<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
 
<style>
 
</style>
</head>
<body>
<div class="container">
<header>
<h1>Building a Home Lab for Active Directory and Cyber Security Training</h1>
<p>This project series will guide you through setting up a comprehensive home lab environment
for learning Active
Directory (AD), Splunk, and security attack simulations.</p>
</header>
<main>
<section>
<h2>Step 1: Introduction and Diagram Creation</h2>
<ul>
<li><strong>Objective:</strong> Understand the project scope and importance of diagramming
your setup.</li>
<li><strong>Steps:</strong>
<ul>
<li>Introduction to Active Directory and its importance in organizations.</li>
<li>Importance of creating a diagram for your lab setup.</li>
<li>Creating a basic network diagram for your home lab.</li>
</ul>
</li>
</ul>
</section>
<section>
<h2>Step 2: Downloading and Installing Assets</h2>
<ul>
<li><strong>Objective:</strong> Set up the necessary virtual machines and software for your
lab.</li>
<li><strong>Steps:</strong>
<ul>
<li>Download and install <a href="https://www.virtualbox.org/">VirtualBox</a>.</li>
<li>Set up <a href="https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022">Windows Server 2022</a>, <a href="https://www.microsoft.com/en-us/software-download/windows10">Windows 10</a>, <a href="https://www.kali.org/downloads/">Kali Linux</a>, and <a href="https://www.splunk.com/en_us/download/splunk-enterprise.html">Splunk on Ubuntu
Server</a>.</li>
<li>Take snapshots of your VMs for easy recovery and experimentation.</li>
</ul>
</li>
</ul>
</section>
<section>
<h2>Step 3: Configuring Sysmon and Splunk</h2>
<ul>
<li><strong>Objective:</strong> Install and configure Sysmon for logging and Splunk for
SIEM.</li>
<li><strong>Steps:</strong>
<ul>
<li>Install <a href="https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon">Sysmon</a> for advanced logging on Windows machines.</li>
<li>Install and configure <a href="https://www.splunk.com/en_us/download/splunk-enterprise.html">Splunk</a> to collect and analyze logs.</li>
<li>Basic Splunk queries and understanding of telemetry data.</li>
</ul>
</li>
</ul>
</section>
<section>
<h2>Step 4: Configuring Active Directory</h2>
<ul>
<li><strong>Objective:</strong> Set up and configure Active Directory on Windows Server.</li>
<li><strong>Steps:</strong>
<ul>
<li>Promote the Windows Server to a domain controller.</li>
<li>Create new domain users and join a Windows 10 PC to the domain.</li>
<li>Basic AD administration tasks.</li>
</ul>
</li>
</ul>
</section>
<section>
<h2>Step 5: Simulating Attacks and Analyzing Telemetry</h2>
<ul>
<li><strong>Objective:</strong> Perform security attack simulations and analyze the results
using Splunk.</li>
<li><strong>Steps:</strong>
<ul>
<li>Use <a href="https://www.kali.org/downloads/">Kali Linux</a> to perform a
brute force
attack on a domain user.</li>
<li>Analyze the telemetry generated from the attack using <a href="https://www.splunk.com/en_us/download/splunk-enterprise.html">Splunk</a>.</li>
<li>Install and use <a href="https://github.com/redcanaryco/atomic-red-team">Atomic Red
Team</a> for additional attack simulations.</li>
</ul>
</li>
</ul>
</section>
</main>
</div>
</body>
</html>
