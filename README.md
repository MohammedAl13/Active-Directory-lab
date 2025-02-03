# Active Directory Home Lab with Windows Server & PowerShell

In this project, we will set up a Windows Active Directory (AD) lab in Oracle VirtualBox to simulate a basic enterprise network. This lab will help you understand Active Directory, networking, and Windows Server management, which are essential skills for cybersecurity and IT professionals. 

This project is made based off of Josh Madakor's Active Directory Lab. All credit goes to him.

**We will:**

✔ Install and configure Windows Server 2019 as a Domain Controller

✔ Create and manage Active Directory Users & Computers (ADUC)

✔ Set up DHCP, DNS, and NAT for network connectivity

✔ Join a Windows 10 Client to the Active Directory Domain

✔ Use PowerShell to automate user creation in AD

This home lab is perfect for learning Active Directory security concepts, testing attacks and defenses, and gaining hands-on experience in a real-world environment.

**Components Used**
------------------------------------------
Oracle VirtualBox – Virtualization software

Windows Server 2019 – Domain Controller

Windows 10 Client – Workstation joined to the domain

PowerShell – Used to automate user creation in AD

Lab Setup
-----------------------------
--> Install Oracle VirtualBox from the official website.

--> Download Windows Server 2019 and Windows 10 ISOs.

--> Install the VirtualBox Extension Pack for better compatibility.

Before starting, we must download and set up a virtual box. This is very straightforward, so I will not provide a step-by-step guide. If you need assistance, watch a short tutorial on YouTube on how to do so. Just ensure it is from Oracle and adjust whatever you need to to meet your computer's standards.

Also, on the same page there will the a download link to the VirtualBox Extension Pack, get that as well.

Getting the Windows 10 ISO file is also quite simple; just follow this link and ensure you get Windows 10, ISO, 64-bit. https://www.microsoft.com/en-us/software-download/windows10

Virtual Machine Setup
--------------------------------------
Now that virtual machine is running, click new and follow the specific configurations.

For this first one, we will name it Domain Controller and in version type, select Other Windows (64-bit). Hit continue.

Give the machine 2GB RAM. (Make sure you have enough on your machine first.) Hit continue.

Then just hit contnue and accept all default configurations.

Before we do anything else, first head on to settings in the domain controller and follow the listed directions:

--> Under Gereral > Advanced tab, chaged shared clip board and Dragn'Drop to be Bidirectional

--> System > Processor, increase bar to 4 CPUs

--> Network > Adapter 1, Check Enable Network adapter box and select, Attached to NAT

--> Network > Adapter 2, repeat the previous step, however selct Internal Network instead of NAT

Now the VM is setup, simply turn on the VM and add the Windows 2019 ISO.

Windows Server 2019 Setup
---------------------------------
![Screenshot 2025-02-03 111337](https://github.com/user-attachments/assets/2116a6a2-bd19-43b6-8a1a-91f4bc67e71e)

Select the desktop experience, otherwise in the others, you will only get CLI.

In installation type, choose Custom install.

After Install is complete, you can create a password and hit next.

After you are logged in, to get enhanced User experence doing this lab, we will need to download VM guest additions. Got to top of your screen to devices and at the bottom, it should say "Insert Guest Additions CD image"

Now go to file explorer > This PC > CD Drive VirtualBox Guest Addition > VBoxWindowsAdditions-amd64

Click and run it and hit next keeping all default settings and at the end, hit install.


















If we head backover to the FTP client desktop, we can see how close each file was captured. For all 3 files, all data and information was 100% preserved. The format is the exact same. The reason why this is now a miniature hack is that nothing we did generated any logs, meaning there is no way to trace that we downloaded/captured any of those files and plaintext login information as they were intercepted in transit. Instead of stealing the information from the server itself, by intercepting in transit, we make it less traceable. This is very important as it holds many real world applications. Someone with ill-intent may find a way to connect there computer to a network and once they run wireshark, they can begin intercepting any passwords, files, packets sent and downloaded onver the network and remain almost invisible. For these reasons, it is important to educate ourselves in Network Security to prevent such incidents from happening both in our local and organization network. One really easy way to keep all your information secure is to encrypt absolutely everything, especially any sensitive information, keep all your firmware up to date and disable insecure and plaintext communication ports like TLS. Taking such precasutions may not stop the threat entirely, but taking such measures greatly reduces the risk of your data being compromised. 





























