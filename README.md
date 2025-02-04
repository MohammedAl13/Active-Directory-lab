# Active Directory Home Lab with Windows Server & PowerShell

In this project, we will set up a Windows Active Directory (AD) lab in Oracle VirtualBox to simulate a basic enterprise network. This lab will help you understand Active Directory, networking, and Windows Server management, which are essential skills for cybersecurity and IT professionals. 

This project is made based on Josh Madakor's Active Directory Lab. All credit goes to him.

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

Also, on the same page, there will be a download link to the VirtualBox Extension Pack; get that as well.

Getting the Windows 10 ISO file is also quite simple; just follow this link and ensure you get Windows 10, ISO, 64-bit. https://www.microsoft.com/en-us/software-download/windows10

Virtual Machine Setup
--------------------------------------
Now that the virtual machine is running click new and follow the specific configurations.

For this first one, we will name it Domain Controller, and in version type, select Other Windows (64-bit). Hit continue.

Give the machine 2GB RAM. (Make sure you have enough on your machine first.) Hit continue.

Then, just hit continue and accept all default configurations.

Before we do anything else, first head on to settings in the domain controller and follow the listed directions:

--> Under General > Advanced tab, change shared clipboard and Dragn'Drop to be Bidirectional

--> System > Processor, increase bar to 4 CPUs

--> Network > Adapter 1, Check Enable Network adapter box and select Attached to NAT

--> Network > Adapter 2, repeat the previous step. However, select Internal Network instead of NAT

Now that the VM is set up, simply turn it on and add the Windows 2019 ISO.

Windows Server 2019 Setup
---------------------------------
![Screenshot 2025-02-03 111337](https://github.com/user-attachments/assets/2116a6a2-bd19-43b6-8a1a-91f4bc67e71e)

Select the desktop experience, otherwise in the others, you will only get CLI.

In the installation type, choose Custom Install.

After installation is complete, you can create a password and hit next.

After you are logged in, we will need to download VM guest additions to enhance the user experience while doing this lab. Go to the top of your screen to devices, and at the bottom, it should say "Insert Guest Additions CD image."

Now go to File Explorer> This PC > CD Drive VirtualBox Guest Addition > VBoxWindowsAdditions-amd64

Click and run it, hit next, keep all default settings, and hit install at the end.

Next, select reboot later and then shut down the VM. After shutting down, start the VM again.

Now go into settings > Networks & Internet > Ethernet > change adapter options. You will see 2 ethernet connections, and now we need to determine which one is Internet facing and which one is internal facing.
To do this, right-click on 1 of them and click status. Now hit details and look at the IPv4 Address. If it starts with 10.0, etc, then this one is Internet-facing; if it starts with 169. etc, then it is internal facing.

![Screenshot 2025-02-03 114734](https://github.com/user-attachments/assets/18e2f356-333d-4833-96c8-b48fad1a744b)

![Screenshot 2025-02-03 114826](https://github.com/user-attachments/assets/a07644fa-18fe-42ec-99f9-97f25e90dc2b)

Now that you have identified them, to easily reference later, rename both Internet and internal to the corresponding connection, or name it to whatever you find makes it easier to identify.

Right-click the Internal Connection and go to properties. Follow the next steps to assign the IP.

![Screenshot 2025-02-03 115438](https://github.com/user-attachments/assets/a1ae52d5-616c-4f92-840f-3191d7faa97a)

![Screenshot 2025-02-03 115608](https://github.com/user-attachments/assets/0f37e536-8484-44bf-bd33-d40dc231771f)

Also, while you are in settings, head over to the About section, rename the PC to Domain Controller, and then restart.

Active Directory Installation
-------------------------------------------
Once the VM is restarted, a popup should come up called Server Manager > Dashboard. We have been ignoring this until now.

You must click on Add Roles and Features and just hit Next. In server selection, you should only have 1 server, so select it and hit next.

In server roles, you will need to click Active Directory domain services. Make sure not to click the others. Then continue clicking next and then install.

![Screenshot 2025-02-03 120208](https://github.com/user-attachments/assets/a43353df-2fd8-477e-88f7-276391870020)

On the top right of the screen, you should see a yellow flag; click it. Click the "Promote this server to a domain controller." Now follow these steps.

![Screenshot 2025-02-03 134435](https://github.com/user-attachments/assets/c6c1f1a0-fc31-410a-861d-e5ba97fac44f)

You can write any name or mydomain.com in the root domain name to make it easier.

On the next screen in the password section, just type the same password for your Windows server. After this, just keep pressing next and then Install. The computer will now restart.

Creating a Personal Domain Account
------------------------------------
Follow the screenshots and directions to set up an account.

![Screenshot 2025-02-03 134840](https://github.com/user-attachments/assets/ef5ac6e0-5fb5-4ee6-9ab2-bec357b79c3e)

![Screenshot 2025-02-03 135050](https://github.com/user-attachments/assets/5017d735-ea68-4eec-8753-f42aeb0c3216)

![Screenshot 2025-02-03 135124](https://github.com/user-attachments/assets/5e3d1a49-707c-4df0-99b3-9fe732364edc)

Name it ADMINS and uncheck the box.

![Screenshot 2025-02-03 135224](https://github.com/user-attachments/assets/9d51d02f-6b18-48ce-962e-e2cdce7d593c)

![Screenshot 2025-02-03 135333](https://github.com/user-attachments/assets/e0d9137f-9b49-4481-b682-5e775284ccad)

Just put your first and last name, and in the user logon name, you can put anything, but the naming convention prefers the Initial of your first name and then last name with 'a-' at the front. EX: John Doe --> a-jdoe

![Screenshot 2025-02-03 135721](https://github.com/user-attachments/assets/8e8b56ff-a33a-4fdd-b3f5-93ccdcea8b33)

Uncheck and recheck the other box, as seen in the above screenshot. Then hit next and finish.

After this, you should see your name on the right console, right-click it, and go to properties. In properties, go to member of and click add.

![Screenshot 2025-02-03 140049](https://github.com/user-attachments/assets/d528a11b-6f5f-4afd-8748-7faf0269ab64)

Type the following and hit check, then click ok, then apply. After doing so, log out.

When logging back in, choose another user at the bottom. The user name will be the a-jdoe you created earlier, and the password will be whatever you set. Keeping all passwords the same throughout this lab will make remembering easier.

Installing RAS / NAT
-----------------------------------------
In the server manager dashboard, click Add Roles and Features. Keep hitting next until you reach the Server roles section. Follow the screenshots.

![Screenshot 2025-02-03 153621](https://github.com/user-attachments/assets/dc4034af-f6c9-4535-9659-9553789670d2)

![Screenshot 2025-02-03 153651](https://github.com/user-attachments/assets/f275dcc5-e2ae-44cc-a400-d2868a602c81)

After doing those 2 configurations, hit next and then install.

Next, navigate to the tools section and do the following:

![Screenshot 2025-02-03 153858](https://github.com/user-attachments/assets/1a42adf2-8662-4e3a-bd34-fcd861328f5b)

![Screenshot 2025-02-03 153955](https://github.com/user-attachments/assets/305bf545-bfd4-4ad1-86b0-66b5565ced01)

![Screenshot 2025-02-03 154012](https://github.com/user-attachments/assets/d3b078fe-3f6a-4917-87e7-958966252be3)

![Screenshot 2025-02-03 154106](https://github.com/user-attachments/assets/20d4c924-e708-4456-80c2-acf9e16a7cc5)

If it doesn't show the network information where we can select the Internet connection, then just hit cancel and redo it. It will come the second time.

Setup DHCP Server
---------------------------------
Click Add Roles and click Next until Server Roles.

![Screenshot 2025-02-04 111616](https://github.com/user-attachments/assets/86e38ab0-138a-42c8-8f6d-a47a260f36a1)

After installing, go here:

![Screenshot 2025-02-04 111916](https://github.com/user-attachments/assets/8b1a2953-947f-4492-9f63-ae2d096111b6)

![Screenshot 2025-02-04 114503](https://github.com/user-attachments/assets/f167e052-8aff-4309-8e71-3fc149327030)

![Screenshot 2025-02-04 114530](https://github.com/user-attachments/assets/4ffeda44-bec2-460b-af94-3f7a1baeced5)

![Screenshot 2025-02-04 114554](https://github.com/user-attachments/assets/02826016-00f9-448c-9ba4-05adbafd024d)

![Screenshot 2025-02-04 114710](https://github.com/user-attachments/assets/3da76cc1-6fbb-4d43-933f-4f6a6f620ad5)

![Screenshot 2025-02-04 114729](https://github.com/user-attachments/assets/29bfb44c-dbba-4aab-9767-8bd562804749)

![Screenshot 2025-02-04 114821](https://github.com/user-attachments/assets/d645aedd-d78b-4958-a651-c2f265a6bf6e)

Make sure to click ADD after typing in the IP Address here.































If we head backover to the FTP client desktop, we can see how close each file was captured. For all 3 files, all data and information was 100% preserved. The format is the exact same. The reason why this is now a miniature hack is that nothing we did generated any logs, meaning there is no way to trace that we downloaded/captured any of those files and plaintext login information as they were intercepted in transit. Instead of stealing the information from the server itself, by intercepting in transit, we make it less traceable. This is very important as it holds many real world applications. Someone with ill-intent may find a way to connect there computer to a network and once they run wireshark, they can begin intercepting any passwords, files, packets sent and downloaded onver the network and remain almost invisible. For these reasons, it is important to educate ourselves in Network Security to prevent such incidents from happening both in our local and organization network. One really easy way to keep all your information secure is to encrypt absolutely everything, especially any sensitive information, keep all your firmware up to date and disable insecure and plaintext communication ports like TLS. Taking such precasutions may not stop the threat entirely, but taking such measures greatly reduces the risk of your data being compromised. 





























