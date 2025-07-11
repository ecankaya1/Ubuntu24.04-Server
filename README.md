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

- There are 2 files that need to be edited here. <br>

### *File 1:*

- The first file is '50unattended-upgrades'. Open this file in the nano text editor by typing 'sudo nano 50unattended-upgrades'. <br>

- In the 'Allowed-Origins' section, check the 3 lines highlighted yellow in the image below aren't commented out. If they were, they would have 2 forward slashes at the start of the line '//', if that is the case, remove the forward slashes. Removing the forward slashes here means security updates will be applied automatically. <br>

![AllowedOrigins](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Allowed%20Origins.png)

- Next, head to the 'Automatic-Reboot' section, this can be found quickly using the search function. Hit 'CTRL-W' & in the search bar type 'automatic-reboot' then hit enter. Remove the forward slashes to uncomment the code & replace 'false' to 'true'. The server has now been told to restart automatically following an unattended upgrade. (The first image below shows you what it will look like upon getting to the code, the next image is the edited code). <br>

![AutoReboot](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Automatic-Reboot.png)

![AutoRebootEdited](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Automatic-Reboot%20corrected.png)

- Thirdly, go to the 'Automatic-Reboot-Time' just below the previous section. Uncomment the code here & change the time for whatever is best for you. (In the real world in should be set to a time where the least users will be active, in my example I set it to 1AM). <br>

![AutoTime](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Auto%20Reboot%20Time.png)

![AutoTimeEdited](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Auto%20Reboot%20Time%20corrected.png)

- To save these changes in nano editor, hit 'CTRL+X' then type 'y' & hit enter. <br>

### *File 2:*

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

- To check the network connections on the server, type 'ip a'. The network adaptors mac address will be listed next to 'link/ether'. <br>

- The network configuration for the server is stored in the '50-cloud-init.yaml' file, located in the '/etc/netplan/' directory. <br>

- This can be opened by typing 'sudo nano /etc/netplan/50-cloud-init.yaml'. <br>

- In this file. you can edit any addresses, settings, etc. <br>

- If any changes were made in the file, after you save and exit, type 'sudo netplan apply'. <br>

## Remote Access

- To remotely access the server on your local network, SSH (Secure Shell) will need to be installed if not already done so. Type 'sudo apt install openssh-server' to check if it has been installed/already installed. <br>

![SSH-Install](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20SSH%20Install.png)

- Next make sure OpenSSH client is installed on your PC. Do this by minimising the Ubuntu server VM & on your PC desktop, navigate to settings > system > optional features & check if OpenSSH client is installed. If not, click 'add features' & simply add it. <br>

![OpenSSH-client](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20SSH%20Windows11.png)

### *Port Forwarding*

- As I am using VirtualBox to run a VM of the Ubuntu server, some extra steps are needed. Open the VirtualBox application & head into the settings of the Ubuntu server VM. Navigate to Network in the Expert Tab, click 'Port Forwarding' on the VM associated adaptor, click 'Add new port forwarding rule', give it a relevant name e.g. SSH & in the protocol colomn, make sure TCP is selected. In host port & guest port, put '22' as port 22 is the default port assigned to SSH. Leave host IP & guest IP blank. <br>

![PortForwarding](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20SSH%20Port%20Forwarding.png) 

### *Remotely Connecting To The Server*

- Open CMD on your PC (The Ubuntu server VM must still be running in the background) & in the command line, type 'ssh' followed by the username of the server account, the '@' symbol then either 'localhost'/'127.0.0.1'/the IP address of the server. It should look like one the following 3: 'ssh emre@localhost', 'ssh emre@127.0.0.1' or 'ssh emre@192.168.1.148'. <br>

![SSH-CMD](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20SSH%20CMD%20Login.png)

(As it will be your first time connecting to the server via SSH, you'll need to type 'yes' to accept the servers fingerprint). <br>

- To log out from a remote session, type 'exit'. <br>

- Connecting via SSH means you can carry out server tasks from any computer on the same network & if planning to run the server headless, now would be the time to disconnect the monitor and keyboard from the physical server. As I am running the server in a VM, I powered off the Ubuntu server in VirtualBox & selected the drop down next to start then clicked headless start. When the server went back online, I connected to the server via SSH in CMD. <br>

![HeadlessStart](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Headless%20Start.png)

## Installing A GUI

- Adding a desktop environment to an Ubuntu server is optional. Keep in mind Ubuntu in its standard form uses far less resources than having a desktop bolted on. <br>

- To keep the load down as much as possible, in this project, I installed a lightweight desktop environment known as LXDE (Lightweight X11 Desktop Environment). <br>

### *Installation*

- In the command line, type 'sudo apt install lxde-core lxappearance' then type 'y' to continue. ('lxde-core' will give a minimal desktop but adding 'lxappearance' will enable you to change the look & feel of the desktop if you wish to do so). <br>

- If a window pops up saying to choose the default display manager (this will provide a graphical login window), to keep it lightweight, highlight 'lightdm' and hit enter. <br>

- Once the installation is complete restart the server by typing 'sudo reboot'. Once restarted you will now see a graphical login window. <br>

![GUI-Login-Window](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20GUI%20Login.png)

- Before logging in, click the Ubuntu logo next to the display name & select 'LXDE'. <br>

![LXDE-Login-Selection](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20GUI%20Login%20LXDE%201.png)

- Log in using the same details as you normally use to log in on the CLI <br>

- To use the CLI, open the LXTerminal application, here you can run commands as you normally would. LXTerminal can be found by opening the start menu > system tools > LXTerminal. <br>

### *Customising The GUI*

- To customize the GUI, click the logo in the bottom left > preferences > Customize Look and Feel. <br>

![GUI-Customise](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20GUI%20Look%20%26%20Feel.png)

### *Removing the GUI*

- To remove the GUI, in the LXTerminal console type 'sudo apt-get remove xserver-xorg-core' hit enter, then type 'y' to continue. Once the command has run, reboot the server. <br>

- (Note that for the following steps in my project, I will be running the server with no GUI). <br>

## RDP (Remote Desktop Protocol)

- In my project I will install XRDP on the server which will allow me to connect to the Ubuntu server from any windows PC on the same network using the 'Remote Desktop Connection' application.

### *Installing XRDP*

- First, in the CLI, type 'sudo apt install xrdp' then type 'y' to continue. Once installed the configuration files need to be edited. <br>

- Type 'sudo nano /etc/xrdp/startwm.sh'. <br>

![XRDP-Config-Before-Any-Edit](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Xrdp%20Config.png)

- Comment out the 2 lines of code just like in the image below by typing '#' at the start of the line. Then Below these lines, create a new line by hitting the enter key, then type 'lxsession -s LXDE -e LXDE' (this line informs xrdp you're connecting to the LXDE desktop'  <br>

![XRDP-Config-Newline](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Xrdp%20Config%20Comment%20%26%20New%20Line.png)

- Save and exit this file by hitting CTRL+X, then typing 'y' and hitting enter. <br>

### *SSL Cert Group*

- Add the xrdp user to the SSL cert group (this needs to be done as Remote Desktop uses an encrypted connection). <br>

- To do this, type 'sudo adduser xrdp ssl-cert' <br>

![SSL-Cert](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Xrdp%20SSL%20Cert.png)

- Restart the server to make sure the changes take effect by typing 'sudo reboot'. <br>

### *Port Forwarding*

- Before an RDP connection can be made, in VirtualBox, in the Ubuntu Server settings, allow port 3389 (Default Port used by the RDP) in port forwarding as previously done with port 22 for SSH. <br>

![Port-Forwarding](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Xrdp%20Port%203389.png)

### *Remote Destop Connection*

- Now, on your windows PC, open 'Remote Desktop Connection' and type in either the IP address of the server, '127.0.0.1' or 'localhost' then click connect. <br>

![Remote-Desktop-Connection](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Xrdp%20RDP%20Localhost.png)

- You will be greeted with the xrdp login screen, enter the login details for your Ubuntu Server on the login window.

![XRDP-Login-Screen](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Xrdp%20RDP%20Login%20Window.png)

- Once logged in, you will now be remotely logged in to the Ubuntu server desktop (you can tell it's remotely logged in due to the blue bar at the top of the screen). <br>

- To exit the remote connection, log out via the start menu or if you want to leave something running on the desktop, you can exit the session with the little cross in the blue bar & return to it later by accessing the server via the Remote Desktop Connection application. <br>

## Web Console

- In this project, I installed a tool called cockpit. Cockpit is a web console that allows you to administrate the server through an easy to use interface. Once cockpit is installed you can access the Ubuntu server from any computer on the local network using a web browser. <br>

### *Installation*

- First, make sure the software package manager is up to date by typing 'sudo apt update'. <br>
- Then install cockpit by typing 'sudo apt install cockpit'. <br>

- Once installed, check cockpit is running by typing 'sudo systemctl status cockpit.socket'. <br>

![Cockpit-Status](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Cockpit%20Status.png)

### *Configuration*

- Cockpits default configuration does not work with Ubuntus network manager, this can be fixed by typing 'sudo nano /etc/netplan/50-cloud-init.yaml'. Look for the line that says 'version: 2' and directly below it, type in 'renderer: NetworkManager', save it by hitting CTRL-X, typing 'y' then hit enter. <br>

![Cockpit-Config](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Cockpit%20NetworkManager.png)

- To make the new configuration take effect, type 'sudo netplan try' then hit enter to accept the new configuration. <br>

- Type 'exit' and note that it now gives the address of the web console on login screen. (You will only see this if using ubuntu without a GUI). <br>

![Web-Console-Address](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Cockpit%20Webconsole%20Login%20Page.png)

### *Port Forwarding*

- Before accessing cockpit on a web browser, first go to the Ubuntu server VM settings in VirtualBox & allow port 9090 in port forwarding as previously done with SSH & RDP. (Cockpit uses port 9090 by default for its web server component). <br>

![Port-Forwarding-Cockpit](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Cockpit%20Port%209090.png)

### *Accessing Cockpit*

- On a your windows PC, go into a web browser & in the search bar type in the IP address of the server followed by a ':' and the port number '9090'. e.g. '192.168.0.200:9090' or using 'localhost:9090' or '127.0.0.1:9090'. <br>

- Once searched, it will say 'Your connection is not private'. Since cockpit uses a self signed certificate, click on advanced then click proceed. <br>

- Once on the login screen, type in your user details to log in. <br>

![Cockpit-Login](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Cockpit%20Login%20Window.png)

![Cockpit-Interface](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Cockpit%20Interface.png)

- You are now able to manage the server, performing tasks such as updating software, managing users, and configuring networks without the need of commands. <br>

## Adding New User Accounts

- In the CLI, type 'sudo adduser new-username' (e.g. 'sudo adduser ecankaya'). <br>

- Enter the password for the new user account. <br>

- Enter the full name of the new user (alternatively you can just hit enter to accept the default) <br>

- Fill in the room number, work phone, home phone, & other with whatever you wish then type 'y' to say the information is correct.<br>

![Add-User-Details](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Adding%20New%20User.png)

- Go to the home directory by typing 'cd /home' and list the contents by typing 'ls', you will see the new account will have its own home directory. <br>

![Add-User-LS](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Adding%20New%20User%20LS.png)

## Adding Administrative Priveleges To User Accounts

- To elevate an account to administrator, it needs to be added to the sudo (SuperUserDO) group. <br>

- In the home directory, type 'sudo usermod -a -G sudo account-username' (e.g. 'sudo usermod -a -G sudo ecankaya'). The account will now be an admin account. Test this by logging into the account and run an admin task. <br>

- I logged into the new admin account via SSH from my windows terminal then run the command 'sudo apt update', the command executed successfully. <br>

![Admin-Account-Command](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Admin%20Privileges.png)

## Switching Users

- Rather than logging out and logging into another account, you can switch accounts. <br>

- In the command line, type 'su - username' (e.g. 'su - ecankaya'). Then enter the password for the account. (You can tell you've switched users as the command prompt will be different or you can confirm this by typing the 'whoami' command). <br>

- Type 'exit' to leave the account. <br>

![Switching-Users](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Switching%20User.png)

## Listing User Accounts

- To list all the user accounts on the server, type 'compgen -u'. <br>

- There will be a lot of system accounts listed, but here you will be able to find any accounts you have created. <br>

## Removing A User Account

- To remove a user account, type 'sudo deluser account-username' (e.g. 'sudo deluser ecankaya'). <br>

- Check it has been removed by typing the 'compgen -u' command. <br>

![Removing-User](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Removing%20A%20User.png)

## Root Account

- In Linux, "root" is the name of the superuser which is a special account for system administration. Switching to this account can be useful if you have to type a lot of admin commands, one after the other as it saves you from having to type 'sudo' each time. <br>

- Do this by typing 'sudo su -' (The dash at the end of the command, isn't necessary, but it enters you into the "root" accounts home directory). <br>

- Type 'exit' to leave the "root" account. <br>

![Switch-Root](https://github.com/ecankaya1/Ubuntu24.04-Server/tree/main/Images)

## Enabling The Root Account

- For security reasons, by default, Ubuntu Server doesn't allow you to login using the "root" account. This is a good security feature, however there may be times when setting up a server, you need the root account to be fully active. <br>

- Type 'sudo passwd root' and create the password for the root account. <br>

- Log out of your account by typing 'gnome-session-quit' then log into the root account using 'root' as the username & the password you set for it. (Whenever you're running as root, the '$' in the command prompt changes to the '#') <br>

![Root-Login](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Root%20Access%20Enable.png)

## Disabling The Root Account

- To remove login access with the root account. <br>

- In the server console type, 'sudo passwd -l root'. <br>

- The root account login will now be disabled. <br>

![Root-Disable](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Root%20Access%20Disable.png)


## Changing The Hostname

- You can see the servers hostname by typing 'hostname' in console. For even more info, type 'hostnamectl'. <br>

![Hostnamectl](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Hostnamectl.png)

- To change the hostname type 'sudo hostnamectl set-hostname new-name' (e.g. 'sudo hostnamectl set-hostname projectserver'). Type 'hostname' to see the servers new name. <br>

![Hostname-Change](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Hostname%20Change.png)

- You will notice the command prompt hasn't updated with the new host name, fix this by typing 'exec bash'. <br>

![Exec-Bash](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Hostname%20Update.png)

As the server boots it maps local hostnames to IP addresses. These are stored in the '/etc/hosts' file. They can be viewed by typing  'cat /etc/hosts'. <br>

![View-Hosts](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Hostname%20cat.png)

- Here you will it still contains the oldname of the server. Update this by typing 'sudo nano /etc/hosts'. Navigate to where your old server name is, delete it with backspace then type in your new server name. Save the file, by hitting CTRL+X, typing 'y' then hitting enter. <br>

- To check the server knows its name, run a ping test using the hostname  e.g. 'ping projectserver'. Hit CTRL+C to cancel the ping. <br>











