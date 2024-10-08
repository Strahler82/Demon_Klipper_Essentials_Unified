# DEMON Armbian HDMI Boot Screen Install

This install requires you to modify system files! If you are uncomfortable doing this please read on & decide if you want to do this. It's not very complex but it does need to be done correctly.

Here you will save a backup of your current boot screen file & replace it with the Demon version. 

## To do this you MUST be running an Armbian system!! No if's buts or coconuts!

MAKE A BACKUP OF YOUR PRINTER &/OR YOUR CONFIG FOLDER!!!!



# This example has been tested WORKING on the STOCK SV08 system

Download the file by clicking the link or pasting the command into your SSH terminal


## Backup Your Current File!

Open your chosen SFTP client & login to your SV08

Navigate to....
```
/usr/lib/firmware/bootsplash.armbian
```

find the `bootsplash.armbian` file already in your system & download it to keep safe as a backup!!! IMPORTANT!!!

## Download The DEMON file

Download the `bootsplash.armbian` file here.

LINK >>>>>>>>>


# install direct via SSH 

This will download the file to your user directory

```
cd ~

wget link
```

## Now to copy it to the correct directory ready for use

Run this command to copy the file to the correct directory
```
sudo cp ~/bootsplash.armbian /usr/lib/firmware/bootsplash.armbian
```

Once that's done its time update the system to use it.
```
sudo update-initramfs -v -u
```
Your pi will run a load of commands that will scroll & there'll be a small wait at the end as it finishes. Once finished it's time to reboot the pi to use the new boot screen!

```
sudo reboot Now
```




## DISCLAIMER - USE WITH CAUTION!!

I am in no way responible for your use of this file &/or if anything bad happens to your printer or system. Running this file is on you, so is any & all results of it.
