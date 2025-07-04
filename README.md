# Ubuntu 24.04 Server Project
### This project simulates a Ubuntu 24.04 Server in a virtualised environment using VirtualBox. This lab builds foundational skills in Linux OS & networking.
<br>

## Objective
This project was created to simulate a basic Ubuntu 24.04 Server in VirtualBox. The goal was to gain a deeper understanding on Linux Systems, the CLI, & basic Linux server setup. By building this lab from the ground up, I aimed to gain hands-on experience with Linux systems & more confidence using the CLI as Linux is a dominant operating system in the server landscape.

## Skills Learned
- Virtualization: Installed and configured Ubuntu 24.04 server on VirtualBox. <br>
- System Setup: Performed initial server setup, including user creation, SSH configuration, and basic security hardening.  <br>
- Networking: Configured VirtualBox networking modes (NAT, Bridged, or Host-Only) for internet access and local connectivity. <br>
- Security: Applied basic server security practices (UFW firewall, disabling root login over SSH, package updates). <br>
- Command Line Proficiency: Gained hands-on experience with Linux terminal commands and system administration tools. <br>
- Package Management: Installed and managed software using apt, systemctl, and service management tools. <br>
- Filesystem Navigation: Learned Linux filesystem structure and how to manage files, directories, and permissions. <br>

## Tools Used
- VirtualBox – Virtualization platform used to run Ubuntu 24.04 server as a guest OS. <br>
- Ubuntu Server 22.04 – Lightweight and stable Linux distribution used for server configuration. <br>
- Terminal / Shell (Bash) – Primary interface for interacting with the server environment. <br>
- SSH – Secure remote access and management of the virtual server. <br>
- UFW (Uncomplicated Firewall) – Simple firewall tool used to manage and configure basic network security. <br>
- APT (Advanced Package Tool) – Package manager used for installing and managing software on Ubuntu. <br>
- Netplan – Network configuration utility for setting up IP addresses and DNS. <br>  
- nano – Command-line text editors used to edit configuration files. <br>

# Steps

## VM Installation

### *1. Install Oracle VM VirtualBox*

Navigate to https://www.virtualbox.org/wiki/Downloads, download the compatible version along with downloading the VirtualBox extension pack. <br>

### *2. Install Ubuntu 24.04 Server ISO*

Navigate to https://ubuntu.com/download/server#manual-install, and install.

## Setting up the server on VirtualBox

- Open VirtualBox. <br>
- Select NEW.<br>
- Give it a name of your choice <br>
- In 'version' drop down select the Ubuntu ISO that was installed.<br>
- Assign it the appropriate amount of RAM & CPU Cores depending on your hardware. <br>
- In settings for the VM (Virtual Machine) set 'Shared Clipboard' and 'Drag'n'Drop' both to bidirectional (this allows for you to copy and paste/drag and drop of files/text from your machine into the VM & vice versa). <br>

## Ubuntu Server Setup

- Go through the Ubuntu server setup, selecting language and keyboard layout. <br>
- On the network connections config, here you can manually assign a static IPv4 address by selecting 'Edit IPv4' & change Automatic(DHCP) to Manual. <br>

![IPv4 Config](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20IPv4%20Config.png)

- Skip configure proxy. <br>
- The mirror address is used to keep the server up to date and the default will be fine. <br>
- For storage, use the virtual storage assigned from VirtualBox. <br>
- Continue through the rest of the installation process & select continue at the 'Confirm destructive action' box. <br>
- On the profile set up page, set your name, the servers name and an admin account with password. <br>

![Profile Setup](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Profile%20Config.png)

- Install SSH (this allows people to connect remotely via SSH & allows the server to be ran in headless mode). <br>

![SSH Install](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Install%20SSH.png)

- On the snap packages page, you can select any software that you want installed with the OS installation (personally I prefer to install the OS first & then add any additional software later). <br>
- Wait for the server installation & when finished select 'Reboot Now'. <br>
- Once rebooted, you will be greated with the login screen, here type in the profile credentials previously created at the profile set up page (the password field will show up blank, this is a security feature). <br>

![Login Screen](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Login.png)

- Once logged in, you will see you're logged in as the user account which was created, in the image below it is 'emre', then following the '@', it will be the name you made for the server, for me it is 'server', after the '~' will represent the directory, which will be the users home directory. This can be confirmed by typing 'pwd' (print working directory). <br>

![Login emre@server](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Login%20emre%40server.png)

![PWD](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20PWD.png)

- To log out of the server, type 'exit'. <br>



 











