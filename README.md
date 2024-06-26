# Full-SOC-Analyst-Lab

![image](https://github.com/MohammedAl13/Full-SOC-Analyst-Lab/assets/154714127/9dfd9fcd-2e36-4179-972a-9700afffe44c)

## Introduction

In this project, I will be building a full SOC environment. To give a brief overview, I will be making a virtual environment, next I will disable all security controls of that environment. Security events generated will be collected and analyzed. Lastly, the enviroment will be hardened and observed again. This project is inspired by Eric Capuano's Blog post, "So you want to be a SOC Analyst? Intro" as well as Gerald Augers video "Uncover the Secrets of a Home SOC Analyst Lab." Since these post are a bit old, ad many of the tools used have since upgraded and/or changed, we will need to make a few changes but it will mostly remain the same. 

This full SOC Home Lab consist of the following components/tools:
- VM Ware
- Ubuntu Virtual Machine
- Windows Virtual Machine
- 

***This home lab will be quite long. We will have to make a few changes to the original lab as certain tools are no longer avaliable. All parts of this lab can be done for FREE.***

To start, we will need to download VMWare to host all of our Virtual Machines.

As of the making of this lab, apparently VMWare has migrated to Broadcom.com and I was unable to find the VMWare workstation needed to do this lab, but I was researching and found a download link to install VMWare Workstation 17 on Reddit. Use this link to begin installation --> https://softwareupdate.vmware.com/cds/vmw-desktop/ws/17.5.1/23298084/windows/core/
- When setting up VMWare, at the end you will be given a choice to try for 30 days or put in a license key, choose the 30 day option. 

![chrome_HBcaPmKm3q](https://github.com/MohammedAl13/Full-SOC-Analyst-Lab/assets/154714127/70ed8e53-1a5b-4976-a88b-c3994e95dd9f)

Now that VMWare is setup, lets get some virtual machines running. For this portion of the lab, we will need to create both a Windows and Ubuntu virtual machine. The link for both of these can be found in Eric Capuano's Part 1 of, "So you want to be a SOC Analyst". 

- To do this, open VMWare and select Create a New Virtual Machine
- 








This miniature SIEM lab consists of the following components:

- Elastic Stack
- Virtualbox
- Kali Linux

***This home lab is not like other home labs as it will be quite short and consist of a few basic steps. All parts of this lab can be done for free.***

Before we can start, a free account must be made on Elastic Stack, as this will host our SIEM, Logs, and dashboard
- Go to https://cloud.elastic.co/registration and create an account to start your free trial
- Next, choose the “Create Deployment” button and select “Elasticsearch” as the deployment type.
- Next, choose the parameters that fit your needs and hit “Create Deployment”
- Once all the configurations are completed, hit continue

Next, we can now build our Virtual Machines. Any Virtualization software and OS can be used for this Homelab, but for the purposes of this lab, I will be using Virtualbox and a Kali Linux Distribution.
- To download a Kali Linux VM, go to https://www.kali.org/get-kali/#kali-virtual-machines and choose which works best for you.
- Next, create a Kali Linux VM in your preferred virtualization software using the Kali Linux file
- Once the Kali VM is installed, click to start the virtual machine, and you will go through a series of screens and prompts, which you may need to fill in some information
- Once everything is complete, you will be asked to log in. Both the username and password are "kali"

**Note, if you face difficulty with installing the Kali Linux VM or setting up the virtualization software, either VMware or Virtualbox, there are a plethora of resources and tutorials on YouTube which can easily be followed**

To collect Logs from our Kali Linux VM, we will need to utilize an Elastic agent. An agent is a tool that collects data from hosts and sends it to the Elastic Stack for monitoring and analysis. Thsi data will be security events collected by the agent and forwarded to the Elastic SIEM. To do this, you must do the following steps:
- Go to the Elastic Home page, and at the left top corner, select the menu bar, and at the bottom, select the "+ Add Integration" button in blue
  
  ![image](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/4a2ea9b5-0e64-4a56-9f52-6c33dc9317cf)

- Next, you must click Elastic Defend and at the bottom, select "Install Elastic Defend"

- You should eventually reach this page

![Install Elastic agent on host](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/9cceecd0-0759-47f1-a264-375f77aca669)

- Depending on which VM you choose, select which one suits you.
- Once copied, head over to your Kali VM and open up the terminal, which is at the top of the screen.
- In terminal, paste your command and hit enter.
- It may ask you for the password, so type "kali"
- Once installed, you should be on this screen

![Elastic agent successfully installed onto kali linux](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/5c27bcd1-666d-4070-ad1c-39191afac0ad)

If you would like to verify whether the agent was successfully installed, you can paste the following command into the terminal: 
sudo systemctl status elastic-agent.service


![check if elastic agent is installed on to kali linux](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/59f673bf-104e-478a-b892-ac952b1ca5df)

Now that everything is running, we can verify the agent is working as intended and generate some security events on the Kali Linux VM. If it works, we should be able to see these events as Logs within the Elastic stack SIEM. There are several ways to do this, but for the sake of this lab, we will be utilizing Nmap. 

Nmap is an open-source tool that facilitates network exploration, management, and security assessments. It identifies devices and services within a network, effectively mapping the network structure. Nmap performs scans to detect open ports, ascertain the operating systems and applications on target systems, and gather various network-related details.

- With Kali Linux, Nmap comes already pre-installed, so if you use a different VM, you may be required to install it. Paste this into terminal to install Nmap if you need to: sudo apt-get install nmap
- Generating a security event using Nmap can easily be done by simply typing this into the terminal: nmap -p- localhost

  ![Generating first query using nmap](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/ae767aab-fe73-461d-a519-3a6bad75b067)

- In addition, you can run several other Nmap scans if you wish to generate a few more events, which will be helpful when we look at the logs. You can paste into terminal: {"nmap -sS <ip address>”, 
  “nmap -sT <ip address>”, “nmap -p- <ip address>”}
- If you do not know the IP address of your VM, this can easily be found using the "ifconfig" command in terminal.

Once you have generated several events using Nmap, we can start analyzing all the logs collected in the Elastic SIEM.
- To do this, you must navigate to the menu bar and, within it, scroll down to Observability, under which the logs button is displayed. You should then reach a screen similar to this.

![image](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/02c702a3-914d-4f87-86a2-818cdb275026)

- These are all the logs you should be getting in your SIEM from your Kali VM
- To see logs about Nmap, you can type process.args: "nmap"

***Now that you have your logs, it is time to build a Dashboard.***
- To do this, click the menu bar, and under the analytics section, select Dashboard
- Now select Create Dashboard and create visualization
- On the right-hand side, select the Visualization type as Bar vertically stacked, the vertical axis as Count, and the horizontal axis as @timestamp.
  
  ![image](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/e9b738c7-398e-4c68-97e0-42c2cbe691a2)

***You don't need to make the Visualization type a Bar vertical stack; it can be whatever you want it to be.***

![image](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/de4ee174-59e6-4471-a8c1-bec7153eb4d6)

The last part of this lab will be creating an alert. Alerts are an integral part of any SIEM as they detect specific security incidents and allow a cybersecurity professional to respond to them promptly. In this lab, we will create an alert in the Elastic SIEM to detect any Nmap scans in the Kali VM. 

- To do this, click the menu icon, and under Security, click Alerts
- Click on manage rules, then click on Create new rules
- Under Define New Rules, select Custom Queue.
  
![image](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/8d08f8ee-2bbb-48ca-af67-63f0e08af8cd)

- Scroll down, and under custom query, type this --> event.action: "nmap_scan"

![image](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/abcbb0c3-0d78-4a32-a859-167b967741f2)

- Next, in the About rule, give your scan a name. Alert names should reflect the purpose of the Alert.
- Also, since the purpose of this lab is to go into some of the responsibilities of an actual SOC Analyst, we should also describe what this Alert is 
  about in detail. In a professional environment, this is important because if another person comes, they should be able to understand the purpose
  behind this Alert quickly.
- Next, assign a severity level to this rule. Since this is the only rule we are making in this lab, I will make it high.
  
![image](https://github.com/MohammedAl13/Elastic-SIEM-Mini-Lab/assets/154714127/7c7f49a4-35ad-4ad6-8c4b-1d22fe15ea64)

- Click continue; now, you can keep all default settings in the schedule rule. Click continue to the next section.
- Finally, in Rule action, you can select what to do when the rule is triggered. You can have an automatic message on Slack, an email, etc. Choose whichever one is 
  convenient to you.
- Lastly, click at the bottom, "Create and enable rule"

Now, whenever you run a Nmap scan on the Kali VM, this alert should be triggered, and if you set up a notification method, you should be notified that this alert has been set off.

## Conclusion

In this project, a Kali Linux Virtual Machine was made, and utilizing Elastic Stack, we integrated an agent onto the Kali Linux VM. The agent forwarded logs and security events from the Kali VM to Elastic Stack's SIEM, where the logs were collected to be analyzed. Next, Nmap scans in the Kali VM were done to generate some security events to be analyzed. A dashboard was also made to view our findings using the collected logs. Lastly, a custom Alert was created to notify you whenever a Nmap scan was conducted on the Kali VM.

## Next Steps

This project can be expanded in various ways. For starters, more logs can be generated, not just from Nmap, but from failed, successful, and remote logs. Next, a different VM could be made, registered in a foreign country, and used to simulate a foreign attack, which would generate several logs and make for an interesting dashboard as we could see the world map and see where attacks are coming from. Another way to explore this lab further is by disabling the firewalls of the Kali VM and exposing it entirely to the internet. Then, using vulnerability management, we can see all the threats, and we could then make incident response and harden the system using NIST 800-53 and compare a before and after. These are just a few suggestions one can implement to maximize their learning experience from this lab.
