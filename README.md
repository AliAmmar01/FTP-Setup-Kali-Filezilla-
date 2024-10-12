#### Introduction:
I installed and set up an FTP server on Kali Linux VM using vsftpd. I then accessed the server using FileZilla from Windows Host and sniffed the network traffic and analyzed it using Wireshark.
#### Task 1:
FTP Server Installation and Set up on Kali Linux VM (Kali 2):

![image](https://github.com/user-attachments/assets/a4c99792-1e04-4cd5-b042-c00276fcecf9)

I installed the vsftpd service which allows hosting an FTP server on “Kali 2” using the command:<br><br>
							***apt-get install vsftpd***<br><br>

I then modified the configuration file for vsftpd (vsftpd.conf) using nano and changed the following options:<br><br>

![image](https://github.com/user-attachments/assets/06acd16f-789f-4af6-a6d0-116d16b17548)

I uncommented the **write_enable=YES** option to enable FTP write commands.<br><br>

![image](https://github.com/user-attachments/assets/a043d4a1-07cc-47a3-9c91-d4a9df17d7d8)

I also uncommented the **chroot_list_enable=YES** option to enable the chroot list functionality & the **chroot_list_file=/etc/vsftpd.chroot_list** option to specify the location of the file that contains a list of users who should be chrooted (restricted to their home directories).<br>
I then created the **vsftpd.chroot_list** file in the **/etc/** directory and added the newcheetos user I have just created specifically for this task, using the command:<br><br>
							***adduser newcheetos***<br><br>

(with the password set as: 1234), to the list of users to chroot.<br><br>
I then started the FTP Server using the command:<br><br>
							***service vsftpd start***<br><br>
							
![image](https://github.com/user-attachments/assets/de3eeeff-8140-46e1-8651-3bbc10d8819d)

and using the command:<br><br>
							***service vsftpd status***<br><br>

![image](https://github.com/user-attachments/assets/355f6364-ccc3-4c08-a623-4ae766df953f)

we can notice that the service is up and running.
To shed the light on a specific example, and after creating a new user, I copied the directory ”Pictures” containing some of the screen captures I used in this report to the **newcheetos** (new user I created) directory to be later transferred to the Windows Host.

![image](https://github.com/user-attachments/assets/8a185d0d-d067-40b9-be52-c62144aaa662)

#### Task 2:
Accessing the FTP Server from a Windows Host via FileZilla and Performing Basic Operations (Login, Upload and Download):
First, let’s find the IP address of the Kali Linux VM (Kali 2) running the FTP Server by running the command:<br><br>
								***ip ad***<br><br>

![image](https://github.com/user-attachments/assets/834785a4-7701-4037-9795-37bdcfe282c9)

As presented in the previous screen capture, the IP address of Kali 2 is 192.168.1.21.
On the Windows Host, I set up the FTP client software, FileZilla and specified the **Host, Username** and **Password** and then pressed **Quickconnect**.

![image](https://github.com/user-attachments/assets/7fee6f08-fc22-4693-9851-3ba5419814a9)

The connection is successful. By a simple drag and drop operation, I transferred this directory to my Windows Host and transferred the “Lights, Camera, Action.pptx” file to the Kali VM to test both the upload and the download FTP functionalities.

![image](https://github.com/user-attachments/assets/27f69e5d-2c1d-4830-845a-d031e4964a35)

View form the Kali VM command line:

![image](https://github.com/user-attachments/assets/e59192e3-ae70-497e-9d8b-beb884f48893)

#### Task 3:
Capturing Network Traffic using Wireshark and Analyzing FTP Protocol:
Simultaneously, I ran Wireshark on the Kali Linux VM on which the FTP Server is running.

![image](https://github.com/user-attachments/assets/e091425c-5a03-4ed5-bad0-110fce4d8a3c)
			
Specify the interface as “eth0” and start sniffing.

![image](https://github.com/user-attachments/assets/d28da821-0d05-4e8a-a71f-450adc1610f1)

The first thing I noticed is that a 3-way TCP Handshake is established before the connection is started.

![image](https://github.com/user-attachments/assets/9d653645-cc54-40ac-9d0c-e3d7d5ca64c9)

This means that the FTP Protocol runs over TCP for reliable file transfer.
The second this I noticed is that the whole FTP conversation was running in passive mode which differs from active FTP mode.
In passive mode:
- The FTP server opens a listening port and tells the client which port to connect to for the data transfer. The server sends a PASV command to the client, indicating the IP address and port number on which the server is listening for data connections.
- The client then initiates a connection to the IP address and port provided by the server. This connection is used for transferring data.
In active mode:
- The FTP client initiates the data connection to the FTP server. The client sends a PORT command to the server, specifying an IP address and port number on which it will listen for incoming data connections.
- The server then initiates a connection from port 20 (the default data transfer port for FTP servers) to the IP address and port number specified by the client. This connection is used for transferring data. <br><br>

![image](https://github.com/user-attachments/assets/5a78deaa-6067-4ee8-9f70-6fbb46c0277e)

The final thing I noticed is that the transferred data is not encrypted and sent across the network in plaintext which is not secure and unsafe in case of sensitive data transfer. In our example, the most sensitive data that I was able to capture was the username (newcheetos) and the password (1234) used to login to the FTP server.<br><br>

![image](https://github.com/user-attachments/assets/3202fc7e-fae0-4c9d-b555-df312562a8b3)
