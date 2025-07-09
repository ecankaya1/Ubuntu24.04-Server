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

## Updates

- Making sure the server is up to date is important for security. This can be done by typing 'sudo apt update' (as this is an administrative command, a password will need to be typed in). <br>

![Apt Update](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Sudo%20Apt%20Update.png)

- Once updated, run the command 'sudo apt upgrade'. <br>
- After running an update, you may want to restart the server which can be done by typing 'sudo reboot'. <br>

## Automatic Updates

- Rather than manually doing updates everytime, you can get the server to automatically do it. <br>
- First, check the unattended upgrades package is installed by typing 'sudo apt install unattended-upgrades' (this package should be installed by default). <br>

![UnattendedUpgrades](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Automatic%20Updates.png)

- As part of the automation process, it can be useful for the server to restart itself to complete the upgrade procedure, to do this, another package has to be installed, by typing 'sudo apt install update-notifier-common'. <br>

![UpdateNotifierCommon](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Automatic%20Updates%20Auto%20Restart.png)

- Next, check the configuration in the APT's config directory. (Advanced Package Tool is a tool for handling the installation & removal of software). do this by typing 'cd /etc/apt/apt.conf.d'. (You can see that you're in the APT directory as the command prompt has changed). <br>
- List the contents of the directory by typing 'ls'. <br>

![APTdirectory](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20APT%20directory.png)

- 2 files that need to be edited in here, the first is '50unattended-upgrades'. <br>
- Open this file in the nano text editor by typing 'sudo nano 50unattended-upgrades'. <br>

### 3 parts to check in this file:

- In the 'Allowed-Origins' section, check the 3 lines highlighted yellow in the image below aren't commented out. If they were, they would have 2 forward slashes at the start of the line '//', if that is the case, remove the forward slashes. Removing the forward slashes here means security updates will be applied automatically. <br>

![AllowedOrigins](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Allowed%20Origins.png)

- Next, head to the 'Automatic-Reboot' section, this can be found quickly using the search function. Hit 'CTRL-W' & in the search bar type 'automatic-reboot' then hit enter. Remove the forward slashes to uncomment the code & replace 'false' to 'true'. The server has now been told to restart automatically following an unattended upgrade. (The first image below shows you what it will look like upon getting to the code, the next image is the edited code). <br>

![AutoReboot](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Automatic-Reboot.png)

![AutoRebootEdited](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Automatic-Reboot%20corrected.png)

- Thirdly, go to the 'Automatic-Reboot-Time' just below the previous section. Uncomment the code here & change the time for whatever is best for you. (In the real world in should be set to a time where the least users will be active, in my example I set it to 1AM). <br>

![AutoTime](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Auto%20Reboot%20Time.png)

![AutoTimeEdited](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Auto%20Reboot%20Time%20corrected.png)

- To save these changes in nano editor, hit 'CTRL+X' then type 'y' & hit enter. <br>

- The second file to check is: '20auto-upgrades'. <br>

- Open it by typing 'sudo nano 20auto-upgrades'. <br>

- In the image below, the first line makes sure that the software package lists are up to date so that the server gets the latest available packages whilst the second line enables the unattended upgrade itself. <br>

![20AutoUpgrades](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%2020auto-upgrades.png)

- The important thing to check in this file is that they're both set to the value of '1' (1 enables the entry & 0 disables the entry). <br>

- Hit 'CTRL-X' to exit the file & if any changes were made, make sure to save. <br>

- With unattended upgrades configured, restart the server by typing 'sudo reboot' so that the changes may take effect. <br>

- Once rebooted, to check the automatic update service is working, type 'sudo systemctl status unattended-upgrades'. <br>

![UpgradesStatus](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Auto%20Update%20Status.png)

## Networking











 











