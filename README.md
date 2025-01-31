# Wireshark-Hacking-Lab

In this project, we will create a Kali Linux virtual machine (VM) alongside two pre-built FTP VMs within VirtualBox to create a controlled hacking environment. We will intercept and analyze packets transmitted between the FTP VMs using Wireshark. The network packets will be files and login information shared between the VMs. Utilizing Wireshark packet capture capabilities no logs nor traceable events will be generated, ensuring that the capture remains undetected. This project aims to explore Wireshark's capabilities in network traffic analysis and evaluate its effectiveness in identifying security vulnerabilities and enhancing network defenses. Also, we will discuss some best practices to protect your organization's information from being easily stolen by Packet Sniffers such as Wireshark at the end.

This Hacking Lab will consist of the following components:
- Oracle Virtual Box
- Kali Linux VM
- 2 ftp pre-built VMs

***This home lab can be done for free. Unfortunately, the files needed for this project are too large to link to this project, and I have tried other methods of importing the files but have been unsuccessful. This lab will just be a walkthrough of what I did.***

Before starting, we must download and set up a virtual box. This is very straightforward, so I will not provide a step-by-step guide. If you need assistance, simply watch a short tutorial on YouTube on how to do so. Just ensure it is from Oracle and adjust whatever you need to to meet your computer's standards.

Once the Virtual Box is set up, we can download and create our Kali Linux VM.

Head on over to Virtualbox, and at the top, select the file and select Import new appliance. Select the Kali Linux ova file and adjust the CPU and RAM as needed. I did not adjust the settings, so I went with 2 CPUs and 2048 MB of RAM. Once this is complete, click import.

Under the description tab, there is the Login information to the Kali VM.

![chrome_rjlL78DNXE](https://github.com/user-attachments/assets/195ab416-d147-46c0-89fb-f738237e61b2)

The next step is downloading and importing the 2 pre-built FTP VMs into Virtualbox. Unfortunately, I was unable to link those files to this project.

After I downloaded the 2 FTP VMs, I then went into the network settings of both to enable Network Adapter and change the network to Host-only Adapter

![chrome_9cYzObhFDt](https://github.com/user-attachments/assets/54901978-5241-476b-aa15-b1f3d6f17ab3)

![chrome_MY13viedJl](https://github.com/user-attachments/assets/bcf66f4b-17fd-495b-8091-9f398c9d7df1)

A very important note: for the lab to work, all three VMs must be on the same exact network. For me, I made sure both FTP VMs were on the VirtualBox Host-Only Ethernet Adapter #3, which is the one my original Kali VM is on. 

After starting both FTP VMs, I went to the FTP server VM first, went into Command Prompt, and typed ipconfig /renew. I am doing this just to make sure everything is clear and good to go with the IP Address. 

![chrome_AfZBFPv4Z9](https://github.com/user-attachments/assets/c50be41f-7205-4491-bc1c-736c441b3a66)

I will now do the same action with the FTP client VM.

![chrome_cbpzzbDpcn](https://github.com/user-attachments/assets/c566acaf-0693-407a-99ba-9e7cbc4600d8)


Important note, the client VM can ping the server, but the server VM cannot ping the client. The reason is that the ICMP firewall is not open on the Client, only on the Server. 

The IP address on the FTP Client VM was 192.168.15.11, and for the Server VM, it was 192.168.15.12. This is very important.

Now, let's ping the FTP server VM from the Client.

![chrome_TsiGg37P22](https://github.com/user-attachments/assets/724c1db3-4f89-45a3-9e76-ecef3ccbfdf3)

The ping was successful, indicating the FTP VMs can communicate with each other. Now we need to make the Kali VM able to communicate as well.

To do this, I will run the ifconfig command in terminal to make sure there is no IP Address conflict.

![chrome_7SwY9vDI3Q](https://github.com/user-attachments/assets/7a1b065d-8350-4b2b-8151-bdf919e432ba)

The IP Address assosiated with this VM is 192.168.15.10, indicating there is no IP conflict.

Next, I will ping the FTP server VM to make sure the Kali VM is able to communicate.

![chrome_k8ttPAT8M7](https://github.com/user-attachments/assets/965c4361-4df1-4f33-b0eb-380a3e1bd6c0)

The ping was successful so now I will go to the FTP client VM and go to FileZilla. Click file and then site manager. In site manager, I willnow change the IP Address to 192.168.15.12, which is the IP of the Server VM. Once that is done, I will hit connect.

![chrome_aZkeiXFSw9](https://github.com/user-attachments/assets/872a059c-5b63-4629-a621-f0b117628dac)

![chrome_NqxAvyQB5f](https://github.com/user-attachments/assets/76885414-7bab-420f-8c30-268a3d3b25fc)

The purpose of this was to verify that communication has been successfully established between the client and Server VM. Now that everything is setup, I can now start the hacking portion of this lab.

Also in Filezila, I can see some files that come with this VM. The information in these files will be used to conduct the hacking portion of the lab. Non eof the information is real, all of it is generated.

![image](https://github.com/user-attachments/assets/66a369bd-7a46-4562-9ca3-2e924740ccc1)

============================================================================================

To start the hacking portion, I will enable promiscuos mode within the Kali VM by going into Settings-->Network-->Advanced

![chrome_U9qNKKK7ze](https://github.com/user-attachments/assets/3ff95cd7-032f-4150-ac9e-45dacb222107)

Enabling promiscuos mode is necessary because of its ability to access all network traffic on a segment. In this case, all the network packets communicated between the VM's on this network.

Now we can start capturing all the packets traveling though the network using Wireshark. In the Kali VM, click applications--> Sniffing and Spoofing--> Wireshark. 

![chrome_Osw9OYrt7e](https://github.com/user-attachments/assets/4626f2af-afa4-4388-a5e5-bf5e358065c5)


Once we reach the Wireshark home screen, we are presented with a few options. eth0 and eth1. Ethernet 0, eth0, is a NAT adapter whereas ethernet 1, eth1, is a VirtualBox only adapter thata both our FTP server and client are using. So we will select eth1 to do the network capture.

![image](https://github.com/user-attachments/assets/3da31c70-9d98-48b3-9b57-752210e02251)


Now that traffic is coming into Wireshark, we will now initiate a connection between our FTP client and server. After a connection is established, the capture will be stopped and we will see if we can find any captured usernames and passwords.

We will navigate to Filezilla on the FTP client and under Fiel --> Site Manager, we will connect to the FTP server.  

![image](https://github.com/user-attachments/assets/b05011a2-c7d6-4d8f-be10-c38db2ec4a1d)

After clicking connect, we can go to the FTP server and see that the connection was made successfully.

![image](https://github.com/user-attachments/assets/1d6b2faf-e2fc-4d69-8df7-6a682e245765)

Now that the connection is made, we will stop the Wireshark capture in our Kali Linux VM. Since there is a lot of information, we will be filtering only for ftp data. If we scroll, we can find a packet stating the Username, password request, and the password in clear plaintext.

![image](https://github.com/user-attachments/assets/25d6ac66-2d6f-4c6c-8abc-e87d81a511a4)

Now that we have successfully captured the Username and Password, we will now capture file contents downloaded over the network.

To do so, make sure all VMs are running and go back into Wireshark and select eth1 and start the packet capture. Then head over to the FTP client and navigate to Filezilla. Like before, we will again connect to the FTP server. Lastly, we will download the 3 files seen in Filezilla by double clicking them.

Once this is complete, we can now end the packet capture in Wireshark.

Since there is a lot of noise, we will filter out the results by typing in the search "ftp of ftp-data". FTP will be the username and password we have already previously captured and ftp-data will be the actual file that was transfered.

If we scroll, we will find some files that were downloaded. A file was downloaded that was named CreditCardinformation.txt.

![image](https://github.com/user-attachments/assets/8ae1cc49-0c85-4e1d-b9d4-e7f7ad39e9f5)

If we click the FTP-data packet under the CreditCardInformation.txt we can see the data that was in the file but it is difficult to read. To make it more readable, we can select the FTP-data and go to File --> Export Pakcet Bytes  and select desktop and name it Credit Card Information.txt and hit save.

We will do the same with the other 2 files.

![image](https://github.com/user-attachments/assets/2a885ef2-5ff3-4408-b24e-e8d890962a4a)

![image](https://github.com/user-attachments/assets/df40c573-a83b-4011-8c48-85b3a7b1c886)

In the end, the Klai Linux VM desktop should look like this.

![image](https://github.com/user-attachments/assets/b54703ec-0acb-41b1-9b06-1b26ca4663c5)

Now we can click each file and see all what was downloaded from the FTP server. 

![image](https://github.com/user-attachments/assets/3a6e7ca2-57d9-4ba5-8f9f-707333f33235)
 
![image](https://github.com/user-attachments/assets/a1c5f7f7-8f43-4978-9403-7589bc29534c)

![image](https://github.com/user-attachments/assets/9a424cc4-e2fa-425a-ba72-b1d60f546772)

If we head backover to the FTP client desktop, we can see how close each file was captured. For all 3 files, all data and information was 100% preserved. The format is the exact same. The reason why this is now a miniature hack is that nothing we did generated any logs, meaning there is no way to trace that we downloaded/captured any of those files and plaintext login information as they were intercepted in transit. Instead of stealing the information from the server itself, by intercepting in transit, we make it less traceable. This is very important as it holds many real world applications. Someone with ill-intent may find a way to connect there computer to a network and once they run wireshark, they can begin intercepting any passwords, files, packets sent and downloaded onver the network and remain almost invisible. For these reasons, it is important to educate ourselves in Network Security to prevent such incidents from happening both in our local and organization network. One really easy way to keep all your information secure is to encrypt absolutely everything, especially any sensitive information, keep all your firmware up to date and disable insecure and plaintext communication ports like TLS. Taking such precasutions may not stop the threat entirely, but taking such measures greatly reduces the risk of your data being compromised. 





























