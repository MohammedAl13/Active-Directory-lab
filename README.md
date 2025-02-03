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

Next select reboot later and then shutdown the VM. After shutting down, start the VM again.

Now go into settings > Networks & Internet > Ethernet > change adapter options. You will see 2 ethernet connections and now we need to determine which one is Internet facing and which one is internal facing.
To do this, simply right click on 1 of them and click status. Now hit details and look at the IPv4 Address. If it starts with 10.0 etc, then this one is Internet facing, if it starts with 169. etc, then it is internal facing.

![Screenshot 2025-02-03 114734](https://github.com/user-attachments/assets/18e2f356-333d-4833-96c8-b48fad1a744b)

![Screenshot 2025-02-03 114826](https://github.com/user-attachments/assets/a07644fa-18fe-42ec-99f9-97f25e90dc2b)

Now that you have identified them, to easily reference later, rename both Internet and internal, to the corresponding connection, or name it to whatever you find makes it easier to identify.

Right click the Internal Connection and go to properties. Follow the next steps to assign the IP.

![Screenshot 2025-02-03 115438](https://github.com/user-attachments/assets/a1ae52d5-616c-4f92-840f-3191d7faa97a)

![Screenshot 2025-02-03 115608](https://github.com/user-attachments/assets/0f37e536-8484-44bf-bd33-d40dc231771f)

Also, while you are in settings, head over to about section and rename the PC to Domain Controller and then restart.

Active Directory Installation
-------------------------------------------
Once VM is restarted, a popup should be coming called Server Manager > Dashboard. We have been ignoring this until now.

You will need to click on Add roles and features and just hit next. Inserver selction, you should only have 1 server so select it and hit next.

In server roles, you will need to click Active Directory domain services. Make sure to not click the others. then continue clicking next and then install.

![Screenshot 2025-02-03 120208](https://github.com/user-attachments/assets/a43353df-2fd8-477e-88f7-276391870020)

On the top right of the screen, you should see a yellow falg, click it. Click the "Promote this server to a domain controller." Now follow these steps.

![Screenshot 2025-02-03 134435](https://github.com/user-attachments/assets/c6c1f1a0-fc31-410a-861d-e5ba97fac44f)

In root domain name, you can write any name you want, or write mydomain.com to make it easier.

On the next screen in the password section, just type in the same password for your windows server. After this, just keep pressing next and then Install. The computer will now restart.

Creating Personal Domain Account
------------------------------------
Follow the screenshots and directions to set account.

![Screenshot 2025-02-03 134840](https://github.com/user-attachments/assets/ef5ac6e0-5fb5-4ee6-9ab2-bec357b79c3e)

![Screenshot 2025-02-03 135050](https://github.com/user-attachments/assets/5017d735-ea68-4eec-8753-f42aeb0c3216)

![Screenshot 2025-02-03 135124](https://github.com/user-attachments/assets/5e3d1a49-707c-4df0-99b3-9fe732364edc)

Name it ADMINS and uncheck box.

![Screenshot 2025-02-03 135224](https://github.com/user-attachments/assets/9d51d02f-6b18-48ce-962e-e2cdce7d593c)

![Screenshot 2025-02-03 135333](https://github.com/user-attachments/assets/e0d9137f-9b49-4481-b682-5e775284ccad)

Just put your first and last name and in the user logon name, you can put anything but naming convention prefers Initial of first name then last name with 'a-' at the front. EX: John Doe --> a-jdoe

![Screenshot 2025-02-03 135721](https://github.com/user-attachments/assets/8e8b56ff-a33a-4fdd-b3f5-93ccdcea8b33)

Uncheck and recheck the other box as seen in above screenshot. Then hit next and finish.

After this, you should see your name on the right console, right click and go to properties. In properties, go to member of and click add.

![Screenshot 2025-02-03 140049](https://github.com/user-attachments/assets/d528a11b-6f5f-4afd-8748-7faf0269ab64)

Type the following and hit check, then click ok, then apply. After doing so, log out.

When logging back in, choose at the bottom other user, and the user name will be the a-jdoe you created earlier and the password will be whatever you set. Keeping all passwords the same throughout this lab will make it easier to remember.

Installing RAS / NAT
-----------------------------------------
In the server manager dashboard, click add roles and features. Keep hitting next until you reach the Server roles section. Follow the screenshots.

![Screenshot 2025-02-03 153621](https://github.com/user-attachments/assets/dc4034af-f6c9-4535-9659-9553789670d2)

![Screenshot 2025-02-03 153651](https://github.com/user-attachments/assets/f275dcc5-e2ae-44cc-a400-d2868a602c81)

After doing those 2 configurations, hit next and then install.

Next, navigate to tools section and do the following:

![Screenshot 2025-02-03 153858](https://github.com/user-attachments/assets/1a42adf2-8662-4e3a-bd34-fcd861328f5b)

![Screenshot 2025-02-03 153955](https://github.com/user-attachments/assets/305bf545-bfd4-4ad1-86b0-66b5565ced01)

![Screenshot 2025-02-03 154012](https://github.com/user-attachments/assets/d3b078fe-3f6a-4917-87e7-958966252be3)

![Screenshot 2025-02-03 154106](https://github.com/user-attachments/assets/20d4c924-e708-4456-80c2-acf9e16a7cc5)

If it doesn't show the network information where we can select the Internet connection, then just hit cancel and redo it. It will come the second time.
































If we head backover to the FTP client desktop, we can see how close each file was captured. For all 3 files, all data and information was 100% preserved. The format is the exact same. The reason why this is now a miniature hack is that nothing we did generated any logs, meaning there is no way to trace that we downloaded/captured any of those files and plaintext login information as they were intercepted in transit. Instead of stealing the information from the server itself, by intercepting in transit, we make it less traceable. This is very important as it holds many real world applications. Someone with ill-intent may find a way to connect there computer to a network and once they run wireshark, they can begin intercepting any passwords, files, packets sent and downloaded onver the network and remain almost invisible. For these reasons, it is important to educate ourselves in Network Security to prevent such incidents from happening both in our local and organization network. One really easy way to keep all your information secure is to encrypt absolutely everything, especially any sensitive information, keep all your firmware up to date and disable insecure and plaintext communication ports like TLS. Taking such precasutions may not stop the threat entirely, but taking such measures greatly reduces the risk of your data being compromised. 





























