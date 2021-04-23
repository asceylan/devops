


#  Kodluyoruz - Trendyol System Infra Bootcamp Case 1 
In this case we have been instructed to do some various Linux system Administration tasks.

These tasks includes :
 * Installation of Centos on Virtual Machine
 * Creating a Personel user on this machine
 * Adding a new disk to the machine
 * Creating a file and putting some text into this file 
 * Finding and moving this text file

##  Installation of Centos on Vm
Installation  process is quite easy. All you have to do is Creating a Vm and then booting the Centos.iso from virtual Dvd which then starts to boot up the iso on vm and starts the Installation process. In the process you have to chose your keyboard layout, which additional softwares you want to install, your NTP and GMT time preferences, your disk and layout and lastly your users and user's passwords.   After your initial setup you can start installing the OS.

## Creating a Personel User 
In my setup `root` was the only user in the system. To create a personel user and to add this user to `sudoers` group  I used these commands: 

       # adduser asim.ceylan
       # passwd asim.ceylan
       # usermod -a -G wheel asim.ceylan

## Adding New Disk to CentOS
To add a new disk to Centos I powered of the system and added a new virtual disk to VirtualBox Config. After that I restarted the OS and checked OS's disk structure with `lsblk`  The new disk could be seen as `/dev/sdb` Than i used these commands to format the Disk and install a file system and lastly mount the disk to the OS:

    sudo fdisk /dev/sdb ( I used n, p, 2, w parameters )
    
    sudo mkfs.ext4 /dev/sdb2
    sudo mkdir /opt/bootcamp && sudo mount /dev/sdb2 /opt/bootcamp
     
Lastly added new configuration for our new disk to `/etc/fstab` for permanent mounting.

## Creating a Text File
For creation of new text file in pointed  I simply used `echo` and `tee : 

    sudo echo 'merhaba trendyol'  | sudo tee -a /opt/bootcamp/bootcamp.txt

## Finding and moving the text file
In the last task we have been asked to move the text file we created from /opt/bootcamp  directory to our home directory while we are in our home directory.  To do that first i checked my current director with `pwd`  . After I ensured  I was in the home directory  I used a `find` command with `-exec` parameter to find and move the text file:

    sudo find / -type f -name 'bootcamp.txt' -exec mv {} . \;
When I checked my home directory with `ll` I could see the command was successful. 

## Screenshots
In Screenshots folder you could see the commands and their outputs.

## License 
 M.I.T License 
