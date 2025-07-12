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

- Here you will see it still contains the oldname of the server. Update this by editing the by typing 'sudo nano /etc/hosts'. Navigate to where your old server name is, delete it with backspace then type in your new server name. Save the file, by hitting CTRL+X, typing 'y' then hitting enter. <br>

- To check the server knows its name, run a ping test using the hostname  e.g. 'ping projectserver'. Hit 'CTRL+C' to cancel the ping. <br>

## Tasksel

- Tasksel is a useful tool that can be added to Ubuntu servers that makee installing popular packages easier. <br>

### *Installation*

- First make sure the servers package manager is up to date with 'sudo apt update'. <br>

- Install tasksel by typing 'sudo apt install tasksel', type 'y' to continue. <br>

- When installed, run it by typing 'sudo tasksel'. <br>

![Tasksel](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Tasksel.png)

- Here you can select from a collection of software, what to easily install. <br>

## LAMP Stack

- A lamp stack consists of a bundle of open source software used for running web applications. (LAMP is an acronym that stands for Linux, Apache, MySQL, PHP). <br>

### *Installation*

- First, ensure all packages are up to date by typing 'sudo apt update' then 'sudo apt upgrade -y' to upgrade existing packages to latest versions. <br>

### *Apache*

- Apache is web-server that will server as the backbone of the LAMP stack. Install it by running 'sudo apt install apache2 -y'. <br>

### *MySQL*

- MySQL is a database management system. Install it by running 'sudo apt install mysql-server -y'. <br>

- It is a good idea to make MySQL secure, do this by running 'sudo mysql_secure_installation'. <br>

(If an error pops up after typing this which states 'Can't connect to local MySQL server through socket /var/run/mysqld/mysqld.sock(2)', uninstall the MySQL by typing 'sudo apt-get remove --purge mysql*' then clean everything else by typing 'sudo rm -rf /etc/mysql' & 'sudo rm -rf /var/lib/mysql' after this run 'sudo apt-get autoremove' then 'sudo apt-get autoclean', once done go back to the 'sudo apt install mysql-server -y' step and start from there). <br>

- It first asks if we want the system to check that we use secure passwords when setting up a database, enter 'y'. <br>

- Then set the policy to enforce this, I selected '2' for strong passwords. <br>

- Now create a password for the 'root' database user, then hit 'y' to continue with the password provided. <br>

- For security, it is a good idea to remove anonymous users. <br>

- Prohibit remote login to any databases by the root user. <br>

- Then remove the test database. <br>

- For those settings to take effect, type 'y'. <br>

### *PHP*

- PHP is a server-side scripting language. <br>

- Run 'sudo apt install php libapache2-mod-php php-mysql -y'. This command installs PHP along with the necessary modules to enable communincation between PHP & MySQL. <br>

### Utilising LAMP

- Once all installed, on a windows PC on the same local network as the Ubuntu server, in a web browsers search bar type in the servers IP address. You will notice you reach the default page for the apache web server. <br>

![Apache-Web-Page](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20LAMP%20Apache%20Default%20Page.png)

- Open CMD on the windows PC & ssh into the ubuntu server. <br>

- Go to the apache servers web directory by typing 'cd /var/www/html' and list the contents of the directory by typing 'ls'. (the 'index.html' file is the Apache2 Ubuntu Default Page you view when you go to the servers IP address in a web browser). <br>

![Apache-Directory](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20LAMP%20Apache%20Directory.png)

- In this directory create a new file by typing 'sudo nano phpinfo.php'. (Being a new file there will be nothing in it). <br>

- In the new file, type in what is typed in the image below. (the '//' makes the line of code a comment which explains the purpose of the file). <br>

![PHPinfo-File](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20LAMP%20PHP%20File.png)

- To save and exit, hit CTRL+X, type 'y' then hit enter. <br>

- Head over to your web browser & in the search bar IP address of the ubuntu server followed by '/phpinfo.php' (e.g. '192.168.1.148/phpinfo.php'). <br>

![PHPinfo-Web-Page](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20LAMP%20PHP%20Site.png)

- This page gives lots of information about the PHP installation. <br>

(While not critical on a home network, if this was an internet facing web server, you wouldn't want to expose this page to anyone who would seek to gain unauthorised access to your server). <br>

- In my project I removed this page. In the apache web server directory, type 'sudo rm phpinfo.php'. <br>

- Now if you go back to the web browser and refresh the phpinfo page, you will see the information is gone. <br>

- Note that to make proper use of the LAMP stack you'll need additional software, what software you use is entirely up to the user but with everything that was just installed, all the groundwork is in place. Many popular open source projects make use of the LAMP stack, including Nextcloud & WordPress. <br>

## Drives

- As I am using VirtualBox to run my Ubuntu Server, I can add drives to my server via the settings in VirtualBox but in the case of running an Ubuntu Server in the real world, I will go through the steps to add, partition, format & mount a drive. <br>

### *Add The Drive*

- Firstly list the drives connected to the server by typing 'lsblk' (stands for 'list block devices'). <br>

![List-Drives](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Drive%20Listed.png)

- Locate the system disk which you want to use. (As my server is a VM it will show few listings compared to a real world machine) On my machine it is 'sda'. <br>

(Note that unless a server has an eMMC drive, your system disk is likely to be 'sda', so the additional drive will be something like 'sdb'). <br>

- You would want to start over by deleting the current partition table signiature. Do this by typing 'sudo wipefs -a /drive/path' (e.g. 'sudo wipefs -a /dev/sda'). It is important to get the drive & path correct as you don't want to do this to the wrong drive. <br>

### *Partition The Drive*

- As it is a server, it is likely that a large drive is going to be used for storing data. For that reason the 'gdisk' command should be used to partition the drive, as this will create a GPT (GUID Partition Table) that is better suited to bigger drives. <br>

- Type 'sudo gdisk /disk/path' (e.g. 'sudo gdisk /dev/sda). Then go through the steps as followed: <br>

- Type 'n' to create a new partition. <br>
- Give it the partition number '1'. <br>
- I want it to start at the beginning of the drive so I hit enter to accept the default. <br>
- To keep things simple, I created a single partition that fills the entire drive, so just hit enter to accept the default. <br>
- Accept the default value for the hex code or GUID by hitting enter. <br>

- With that done, write the changes to the disk by typing 'w', then typing 'y' to proceed. <br>

- To see the new partition, type 'sudo gdisk -l /file/path' (e.g. 'sudo gdisk -l /dev/sda'). <br>

![Drive-Partition](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Drive%20New%20Partition.png)

### *Format The Drive*

- Now that there is a partition, a file system needs to be created on it. <br>

- In windows, FAT32 & NTFS is used to format drives but as this is Linux, I used Ext4. <br>

- Type 'sudo mkfs.ext4 /partition/path' (e.g. 'sudo mkfs.ext4 /dev/sda1'). (If you gave it a partition of 1, then it would be sda1). <br>

### *Mount The Drive*

- Unlike windows, Linux doesn't use drive letters like C Drive or D Drive, instead the OS is located in the root directory, when an additional drive is added to make it accessable, it needs to be mounted within the root directory. There is an '/mnt' (mount) folder for just this purpose. <br>

- Navigate to the mount folder by typing 'cd /mnt'. <br>

- Here a mount point needs to be created, this is a folder that will mount a drive & you can use the 'mkdir' (make directory) command to do this. <br>

- Type 'sudo mkdir mount-point-name' (e.g. 'sudo mkdir drive2'). <br>

- Run 'ls' command to see the new folder. <br>

- Go into the folder by typing 'cd drive2'. <br>

- If a file is created in here, it is stored on the primary OS drive, that's because although a mount point has been created, the 2nd drive hasn't been mounted to it yet. <br>

- I created a file inside the drive2 folder called 'test' by typing 'sudo touch test', then removed it using 'sudo rm test'. <br>

- Return to the previous directory by typing 'cd ..' and make the mount point immutable (this is so if the storage drive doesn't mount, you don't want all the files ending up on the disk that contains the OS). Do this by typing 'sudo chattr +i drive2'. <br>

- Go back into drive2 by typing 'cd drive2' then attempt to make a file again by typing 'sudo touch test' & you will see that the operation is not permitted. <br>

- In the image below you will see all the commands used in the steps I took in mounting the drive. (Note that you do not need to create a test file, I did it to show the process). <br>

![Drive-Mount-Steps](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Drive%20Mount.png)

- When you mount the drive, you could use its dev path & partition number e.g. '/dev/sda1'. The issue with this method is that if at some point the server gets taken apart & the disks get moved around, there's no guarantee when you put the server back together, the drives would get the same label. A much more reliable way is to use UUID. <br>

### *UUID (Universally Unique Identifier)*

- To find the UUID of your drive, in the '/mnt' directory, type 'sudo blkid'. <br>

- Next to the partition that was created, you will see the UUID beside it. Highlight & copy the UUID with 'CTRL+C' (I used SSH from my windows terminal as it easier to copy). With that copied, you now will go to edit a file. <br>

![Drive-UUID](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Drive%20UUID.png)

- Type 'sudo nano /etc/fstab'. (You can manually mount your drive, but it would have to be manually added every time the server is rebooted, so by adding an entry to the 'fstab' file it will mount automatically whenever the server boots). <br>

- In the 'fstab' file, head to the bottom and hover the mouse over the window, then right click to paste the UUID to the right of that line add '/mnt/drive2 ext4 defaults 0 2'. (That will take the additional drive, mount it to the mount point that was created using the ext4 file system with the default mount settings. the '0' & '2' stand for dump & fsck (file system consistency check)).

Save this file using CTRL+X, typing 'y' then hitting enter.

To test the new entry works correctly (an error in the fstab file can prevent the server from starting), type 'sudo mount -a'. Running that command has mounted any drives listed in the 'fstab' file. From now on, this will be done whenever the server boots.

## File Permissions

- Typing 'ls -l' will give more info on the listings within the current directory. In the image below you will see everything in the directory is owned by 'root', that's why you constantly have to use the 'sudo' command (accessing data owned by 'root' required sudo privileges). <br>

![File-Permissions-LS](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20File%20Permissions%20LS%20-L.png)

- To own these, go into the parent directory by typing 'cd ..' then type 'sudo chown -R username directory/'. Go back into the directory you changed the permissions of, list the contents again with 'ls -l', you will now see that you own the files within the given directory. <br>

![File-Permissions-CHOWN](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20File%20Permissions%20CHOWN.png)

## User Access

- The letters and dashes highlighted in the image below visually identify a files permissions & are arranged as: <br>

![User-Access-RWX](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20User%20Access%20File%20Permissions.png)

![User-Access-File-Permissions](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20User%20Access.png)

- The test file in the image shown has read/write assigned to the owner & read assigned to both group & others. ('r' stands for read, 'w' stands for write, 'x' stands for execute). <br>

- While you can change permissions for an individual file, it's more realistic to change the permissions for an entire directory. Do this by returning to the parent directory using 'cd ..' then type 'sudo chmod -R 750 directory/'. The '750' in the command represents the octal notation of read/write/execute. <br>

![User-Access-chmod](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20User%20Access%20Changed%20Perms.png)

- The numbers shown in the image below represent the OWNER/GROUP/OTHERS respectively which is known as Octal Notation. <br>

![Octal-Notation](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20User%20Access%20Octal%20Notation.png)

- Go back into the directory which you changed user access in, type 'ls -l'. You can now see that the permissions have changed to what you set it as. <br>

## File Sharing

- There are a few ways to share files over a network, e.g. NFS & SFTP. In this project I used a software called Samba (which uses SMB & CIFS protocol). <br>

### *Samba*

- Firstly before installing Samba, check the package manager is up to date using 'sudo apt update'. <br>

- Install Samba by typing 'sudo apt install samba'. <br>

- With the software installed, it now needs to be configured. <br>

### *Adding A Samba User*

- Use an existing account on the server & add it to samba by typing 'sudo smbpasswd -a username' & give it a password of your choosing. <br>

![Adding-A-Samba-User](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Samba%20New%20User.png)

### *Creating A Samba Share*

- Go into the samba directory by typing 'cd /etc/samba'. <br>

- In here you're going to edit the 'smb.conf' file but before that happens, make a backup just in case by typing 'sudo cp smb.conf smb.bk'. <br>

- Edit the 'smb.conf' file by typing 'sudo nano smb.conf' & jump to the end of the file by hitting CTRL+END. Here you will type in something similar to the image below. <br>

![Samba-sbm.conf](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Samba%20Share.png)

- The '#' line is a comment on what the code does. <br>
- The '[share]' line is the name of the share. <br>
- The 'path = /mnt/drive2' line is the path to the directory which is going to be shared on the network. <br>
- The 'valid users = emre' line is the username of the samba account that we want added to the share. <br>
- The 'read only = no' line is whether the user can write on the drive, if you want the user to be able to write, type 'no'. <br>

- To save hit CTRL+X, type 'y' then hit enter. <br>

- With the configuration saved, the samba service needs to be restarted for it to take effect. Do this by typing 'sudo systemctl restart smbd.service'. <br>

- Check the samba configuration for errors by typing 'testparm'. <br>

![Samba-Testparm](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Samba%20Testparm.png)

- Hit enter and you will see the samba share that was created at the bottom. <br>

![Samba-Testparm-Dump](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Samba%20Testparm%20Dump.png)

### *Accessing The Network Files On Windows*

- On a windows PC, go into file explorer & in the address bar type '\\server-ip-address\share-name' (e.g. \\192.168.1.148\share). <br>

![Samba-Address-Bar](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Samba%20Windows%20Access.png)

- A login box will pop up, put in the username & password of the samba account & you will now have access to the shared drive from a windows PC. <br>

![Samba-Windows-Login](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20Samba%20Windows%20Access%20Files.png)

## UFW (Uncomplicated Firewall)

- Ubuntu servers come with a built in firewall but by default it is turned off, this can be seen by typing 'sudo ufw status' <br>

![UFW-Status](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20UFW%20Status.png)

- Before turning the firewall on, be careful if you're logged into the server remotely as you can lock yourself out. (This is because by default the firewall will only allow outgoing traffic). <br>

- To overcomes this, ensure SSH access is allowed by adding a firewall rule. <br>

### *Firewall Rules*

- To allow ssh access through the firewall, type 'sudo ufw allow ssh'. <br>

- With that done, turn on the firewall by typing 'sudo ufw enable'. (Test this by going over to CMD on a windows PC and log into the server via ssh). <br>

![UFW-Allow-SSH](https://github.com/ecankaya1/Ubuntu24.04-Server/blob/main/Images/Ubuntu%20UFW%20SSH%20Rule%20And%20Enable.png)

- You can also remove ssh access by typing 'sudo ssh deny ssh'. <br>

With an active firewall you need to think about all the connections to the server even on your local network. For example if network share is set up with samba, you need a firewall rule to allow Samba. <br>

- This can be overcome by typing 'sudo ufw allow samba', now you will able to access server files from a windows PC. <br>

- If you want to disable the firewall by typing 'sudo ufw disbale'. <br>



























