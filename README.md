<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">


 
</head>
<body>
<div class="container">
<header>
<h1>Building a Home Lab for Active Directory Splunk and AtomicRed </h1>
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

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
<body>
<div class="container">
    <header>
        <h1>Project Diagram</h1>
        <p>This section focuses on creating a network diagram for our Active Directory project using
        <a href="https://www.draw.io/">draw.io</a>.</p>
    </header>
    <main>
        <section>
            <h2>Building the Diagram</h2>
            <p>We will use the following components in our diagram:</p>
            <ul>
                <li><strong>Splunk Server</strong>: <span>192.168.10.10</span></li>
                <li><strong>Active Directory Server</strong>: <span>192.168.10.20</span></li>
                <li><strong>Target Machine (Windows 10)</strong>: DHCP assigned</li>
                <li><strong>Attacker Machine (Kali Linux)</strong>: <span>192.168.10.250</span></li>
                <li><strong>Switch</strong>: To connect all devices within the network</li>
                <li><strong>Router</strong>: To connect the internal network to the internet</li>
                <li><strong>Cloud</strong>: To represent the internet</li>
            </ul>
        </section>

   <a href="https://ibb.co/MBN9j6m"><img src="https://i.ibb.co/CP0WX2f/Diagram.png" alt="Diagram" border="0"></a>

 
</div>
</body>
</html>

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


</head>
<body>
<h2>Install and Configure Sysmon and Splunk</h2>
<p>Objective: Install and configure Sysmon and Splunk on the Windows Target machine
and Windows Server to collect telemetry data and send logs to the Splunk server.</p>
<h3>Setting up NAT Network in VirtualBox</h3>
<pre>
1. Create a NAT Network in VirtualBox (Tools > Network > NAT Network > Create)
2. Configure virtual machines to use the NAT Network
- Settings > Network > Adapter > NAT Network
- Select the created NAT Network name
3. Set static IP for the Splunk server (e.g., 192.168.10.10)
- Edit /etc/netplan/installer.yaml
- Set addresses, nameservers, and routes
- Run 'sudo netplan apply'
- Verify IP with 'ip a'
</pre>
<h3>Install Splunk on Splunk Server</h3>
<pre>
1. Download Splunk installer (splunk.com > Products > Free Trials and Downloads)
2. Install Splunk on the Splunk server virtual machine
- Run 'sudo dpkg -i splunk-*.deb'
- Configure admin username and password
3. Enable Splunk to run on boot
- 'sudo /opt/splunk/bin/splunk enable boot-start'
</pre>
<h3>Install Splunk Universal Forwarder and Sysmon on Target Machine</h3>
<pre>
1. Download and install Splunk Universal Forwarder
2. Download and install Sysmon
3. Configure inputs.conf for Splunk Forwarder
- Specify data sources (e.g., Security, Application, System, Sysmon)
- Define index (e.g., 'endpoint')
4. Restart Splunk Forwarder service after config changes
</pre>
<h3>Configure Splunk Server</h3>
<pre>
1. Create 'endpoint' index in Splunk
- Settings > Indexes > New Index
2. Enable receiving port (9997) for Splunk server
- Settings > Forwarding and Receiving > Configure Receiving > New Receiving Port
3. Verify data in 'endpoint' index
- Search &amp; Reporting app > index=endpoint
</pre>

<a href="https://ibb.co/cc5Gx2r"><img src="https://i.ibb.co/99XFGyr/splunk-static-ip-adress.png" alt="splunk-static-ip-adress" border="0"></a>
 

 
</head>
<body>
<h2>Commands Explained</h2>
<h3>Setting up static IP on Splunk server:</h3>
<pre>sudo nano /etc/netplan/installer.yaml
# Edit addresses, nameservers, routes
sudo netplan apply
ip a</pre>

 
<p>Explanation: These commands are used to set a fixed IP address for the Splunk
server. The <code>nano</code> command opens a text file for editing, where you can specify
the IP address, DNS server, and network route details. The <code>netplan apply</code>
command applies the changes, and the <code>ip a</code> command displays the server's IP
address to verify the changes.</p>
<h3>Installing Splunk on Splunk server:</h3>
<pre>sudo dpkg -i splunk-*.deb
sudo /opt/splunk/bin/splunk enable boot-start</pre>
<p>Explanation: The first command installs the Splunk software on the server. The
second command sets up Splunk to start automatically when the server boots up.</p>

<a href="https://ibb.co/StSSYgk"><img src="https://i.ibb.co/k4ZZbTL/command-to-enable-splunk-autostart-on-reboot.png" alt="command-to-enable-splunk-autostart-on-reboot" border="0"></a>

<h3>Mounting shared folder on Splunk server:</h3>
<pre>sudo mount -t vboxsf -o uid=1000,gid=1000 active-directory-project
/path/to/share</pre>
<p>Explanation: This command allows the Splunk server to access files and folders
shared from the host machine (your computer). It "mounts" the shared folder to a specific
directory on the server.</p>
<h3>Installing Sysmon on the Target machine:</h3>
<pre>cis1mon64.exe -i C:\path\to\sysmon_config.xml</pre>
<p>Explanation: This command installs Sysmon (a system monitoring tool) on the Target
machine (a Windows computer) using a specific configuration file.</p>
<h3>Configuring inputs.conf for Splunk Forwarder:</h3>
<pre># Create inputs.conf in C:\Program Files\Splunk\etc\system\local
# Paste the inputs.conf content</pre>
<p>Explanation: This step involves creating a configuration file (inputs.conf) in a specific
location on the Target machine. The file specifies what types of system logs and events should
be collected and sent to the Splunk server.</p>
<h3>Restarting Splunk Forwarder service:</h3>
<pre># Open Services
# Restart 'Splunk Forwarder' service</pre>
<p>Explanation: After making changes to the configuration file, the Splunk Forwarder
service needs to be restarted. This command can be executed by opening the Services
application on the Windows machine and restarting the "Splunk Forwarder" service.</p>
<h3>Creating 'endpoint' index in Splunk:</h3>
<pre># Splunk Web UI
Settings &gt; Indexes &gt; New Index
Name: endpoint</pre>
<p>Explanation: In the Splunk web interface, this step creates a new index called
"endpoint" where the collected data from the Target machine will be stored.</p>
<h3>Enabling receiving port (9997) on Splunk server:</h3>
<pre># Splunk Web UI
Settings &gt; Forwarding and Receiving &gt; Configure Receiving &gt; New Receiving Port
Port: 9997</pre>
<p>Explanation: This step configures the Splunk server to listen for incoming data on a
specific port (9997). It allows the Target machine to send data to the Splunk server.</p>
<h3>Verifying data in 'endpoint' index:</h3>
<pre># Splunk Web UI
Search &amp; Reporting app
index=endpoint</pre>
<p>Explanation: After setting up everything, this command in the Splunk web interface
allows you to search and view the data collected from the Target machine, stored in the
"endpoint" index.</p>
<a href="https://ibb.co/hVtgr6z"><img src="https://i.ibb.co/gJBrQ2K/splunk-endpoint.png" alt="splunk-endpoint" border="0"></a>

</body>
</html>
///////////////////////////////////////////////////////////////////////

</head>
<body>
<h2>Install and Configure Active Directory</h2>
<p>Objective: Install and configure Active Directory on the server, promote it to a domain
controller, and configure the Target machine to join the newly created domain.</p>
<h3>Set static IP on the server:</h3>
<pre>Right-click network icon > Open Network and Internet settings
Change adapter options > Right-click interface > Properties
Internet Protocol Version 4 > Use the following IP address:
IP address: 192.168.10.7
Subnet mask: 255.255.255.0
Default gateway: 192.168.10.1
DNS: 8.8.8.8
</pre>
<p>Explanation: These steps configure a static IP address for the server, allowing it to
communicate with other machines on the network.</p>
<h3>Install Active Directory Domain Services:</h3>
<pre>Server Manager > Manage > Add Roles and Features
Select Active Directory Domain Services
Click Install
</pre>
<p>Explanation: This process installs the Active Directory Domain Services (AD DS) role
on the server, enabling it to function as a domain controller.</p>
<h3>Promote server to a domain controller:</h3>
<pre>Server Manager > Flag icon > Promote This Server to a Domain Controller
Add a new Forest (new domain)
Domain name: Admin.local
Set Administrator password
Install
</pre>
<p>Explanation: These steps promote the server to a domain controller, creating a new
Active Directory domain called "Admin.local". The administrator password is set during this
process.</p>
<h3>Create users and organizational units:</h3>
<pre>Active Directory Users and Computers
Right-click domain > New > Organizational Unit
Name: IT, HR
Right-click Organizational Unit > New > User
User: Jenny Smith (username: jsmith)
User: Terry Smith (username: tsmith)
</pre>
<p>Explanation: Organizational units are created to group users and resources. Two
sample users (Jenny Smith and Terry Smith) are created within the IT and HR organizational
units.</p>
<h3>Join Target machine to the domain:</h3>
<pre>On Target machine:
System Properties > Advanced > Computer Name > Change
Domain: Admin.local
Set DNS server to the domain controller IP (192.168.10.7)
Enter Administrator credentials
Log in as Jenny Smith (jsmith)
</pre>
<p>Explanation: The Target machine is joined to the "Admin.local" domain by changing
its computer name and DNS settings. The Administrator credentials are used to authorize the
join process. After joining, the user Jenny Smith can log in to the Target machine using her
domain credentials.</p>
</body>
</html>

  
<head>
 </head>
<body>
    <h1>Brute force attack and Atomic Red</h1>
    <hr style="border-color: red;">
</body>
</html>


<p>Objective: Use Kali Linux to perform a brute force attack on user accounts, and set up
Atomic Red Team on the
Windows Target machine to generate telemetry data for testing detection capabilities.</p>
<h3>Set up static IP on Kali Linux:</h3>
<pre>Right-click network icon > Edit Connections
Select Wired Connection profile
IPv4 Settings > Manual
IP address: 192.168.10.250
Netmask: 255.255.255.0
Gateway: 192.168.10.1
DNS: 8.8.8.8
Click Save
</pre>
<p>Explanation: These steps configure a static IP address for the Kali Linux machine, allowing
it to communicate with
other machines on the network.</p>
<h3>Prepare for brute force attack:</h3>
<pre>sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install crowbar
mkdir ad-project
cp /usr/share/wordlists/rockyou.txt.gz ad-project/
cd ad-project/
gunzip rockyou.txt.gz
head -n 20 rockyou.txt > passwords.txt
nano passwords.txt
# Add your target password at the end
</pre>
<a href="https://ibb.co/k5tc7mn"><img src="https://i.ibb.co/LY3pFNw/crowbar-unzip-rockyou-cp-to-ad-project.png" alt="crowbar-unzip-rockyou-cp-to-ad-project" border="0"></a>
<a href="https://ibb.co/brkg8JB"><img src="https://i.ibb.co/tb9MWXs/add-manual-password.png" alt="add-manual-password" border="0"></a>

<p>Explanation: These commands update Kali Linux, install the crowbar brute force tool,
create a directory for the
project, copy the rockyou wordlist, extract it, create a smaller password list (first 20 lines), and
add your target
password to the list(picture2).</p>
<h3>Perform brute force attack:</h3>
<pre>crowbar -b rdp -u tsmith -C passwords.txt -s 192.168.10.100/32</pre>
<a href="https://ibb.co/vkSzd4M"><img src="https://i.ibb.co/DQZKMfj/crowbar-attack-rdp-protocol-user-password-list-and-specific-address.png" alt="crowbar-attack-rdp-protocol-user-password-list-and-specific-address" border="0"></a>
<a href="https://ibb.co/j6FChNc"><img src="https://i.ibb.co/dWyNBTd/successfull-rdp-login.png" alt="successfull-rdp-login" border="0"></a>
<a href="https://ibb.co/mTWZDTw"><img src="https://i.ibb.co/1QWyvQ5/successfull-loggon.png" alt="successfull-loggon" border="0"></a>
<p>Explanation: This command uses the crowbar tool to perform a brute force attack on the
RDP (Remote Desktop
Protocol) service, targeting the user account "Ramilnamazov" with the password list "passwords.txt,
 Second picture shows how it was successfull in bruteforce ,(password blurred) , we can go head and check failed request , EventCode:4624"
and the target IP
address "192.168.10.100".</p>
<h3>Install Atomic Red Team:</h3>
<pre>On Target machine:
Run PowerShell as Administrator
Set-ExecutionPolicy Bypass -Scope CurrentUser
Install-Module -Name Atomic-Red-Team
Exclude C:\ drive from Windows Defender
 <a href="https://ibb.co/hFC7MqF"><img src="https://i.ibb.co/CKW1trK/install-Atomic-Red-Teams.png" alt="install-Atomic-Red-Teams" border="0"></a>
<a href="https://ibb.co/7gdPwMW"><img src="https://i.ibb.co/Q83L2SN/set-exclusion-for-C-drive-for-atomic-red.png" alt="set-exclusion-for-C-drive-for-atomic-red" border="0"></a><br /><a target='_blank' href='https://dedupelist.com/'>find duplicates online</a><br />

 
</pre>
<p>Explanation: These steps install the Atomic Red Team framework on the Windows Target
machine, which allows you to
simulate various attack techniques and generate telemetry data for testing detection
capabilities. The C:\ drive is
excluded from Windows Defender to prevent removal of Atomic Red Team files.</p>
<h3>Run Atomic Red Team tests:</h3>
<pre>Invoke-AtomicTest T1136.001
# Creates a local user account
Invoke-AtomicTest T1059.001
# Executes a PowerShell command
</pre>
<a href="https://ibb.co/BqNFV53"><img src="https://i.ibb.co/xJHdStL/powershell-scripting.png" alt="powershell-scripting" border="0"></a>
<a href="https://ibb.co/XsRhS75"><img src="https://i.ibb.co/jMXcgyw/Atomics-On-C-Drive.png" alt="Atomics-On-C-Drive" border="0"></a>

<p>Explanation: These commands run specific tests from the Atomic Red Team framework.
The first test creates a local
user account, and the second test executes a PowerShell command. You can then check
Splunk for the generated
telemetry data and verify if your detection mechanisms are working correctly.</p>
<a href="https://ibb.co/hVtgr6z"><img src="https://i.ibb.co/gJBrQ2K/splunk-endpoint.png" alt="splunk-endpoint" border="0"></a>
</body>
</html>

