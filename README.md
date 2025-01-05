# Raspberry Pi Zero USB Recovery Tool

This project transforms a Raspberry Pi Zero into a powerful USB gadget for system recovery, network adapter functionality, or storage device.

## Features
- **USB Mass Storage**: Act as a bootable storage device.
- **Network Adapter**: Share internet or access the Pi via SSH.
- **System Recovery**: Includes tools like `testdisk` and `ddrescue` for recovery tasks.
- **Custom Scripts**: Automate recovery and maintenance processes.
- **Interactive Menu**: Easily switch between functionalities.

---

## Prerequisites
1. **Raspberry Pi Zero (or Zero W)** with a microSD card.
2. A computer with USB ports.
3. A micro-USB data cable (not just for charging).
4. Latest **Raspberry Pi OS Lite** installed.

---

## Installation

### Step 1: Prepare the Raspberry Pi Zero
1. **Flash Raspberry Pi OS Lite**:
   - Use the [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to install Raspberry Pi OS Lite onto your microSD card.
   - Enable SSH by creating an empty `ssh` file in the `boot` partition.

2. **Enable USB OTG**:
   - Edit `config.txt` on the boot partition:
     ```txt
     dtoverlay=dwc2
     ```
   - Edit `cmdline.txt` and add the following after `rootwait`:
     ```txt
     modules-load=dwc2,g_ether
     ```

3. **Boot the Raspberry Pi Zero**:
   - Insert the microSD card and connect the Pi Zero to your computer via its USB port (not the power-only port).

---

### Step 2: Configure USB Gadget Modes

#### USB Mass Storage Mode
1. **Create a virtual disk**:
   ```bash
   dd if=/dev/zero of=/piusb.bin bs=1M count=1024
   mkfs.vfat /piusb.bin


2. **Enable mass storage gadget**:
modprobe g_mass_storage file=/piusb.bin stall=0 removable=1



3. **Make it persistent**:
Add the following to /etc/rc.local before exit 0:
   ```bash
modprobe g_mass_storage file=/piusb.bin stall=0 removable=1




USB Network Adapter Mode
Enable network gadget:
   ```bash
modprobe g_ether


Share internet:

Configure internet sharing on your host computer to provide network access to the Pi Zero.





Step 3: Install Recovery Tools
Update and install tools:

bash
Copy code
sudo apt update
sudo apt install parted gparted rsync ddrescue testdisk
Set up SSH:

Enable SSH to access the Pi remotely:
bash
Copy code
sudo systemctl enable ssh
Add custom recovery scripts:

Example:
bash
Copy code
#!/bin/bash
rsync -a /source/ /backup/
Step 4: Add an Interactive Menu (Optional)
Install whiptail:

bash
Copy code
sudo apt install whiptail
Create a menu script:

bash
Copy code
#!/bin/bash
OPTION=$(whiptail --title "Pi Zero USB Recovery Tool" --menu "Choose your option" 15 60 4 \
"1" "Network Adapter" \
"2" "Storage Gadget" \
"3" "Disk Recovery" 3>&1 1>&2 2>&3)

case $OPTION in
    1)
        modprobe g_ether
        ;;
    2)
        modprobe g_mass_storage file=/piusb.bin stall=0 removable=1
        ;;
    3)
        testdisk
        ;;
esac
Save the script as menu.sh and make it executable:

bash
Copy code
chmod +x menu.sh
Run the menu:

bash
Copy code
./menu.sh
Step 5: Test the System
Mass Storage:
Connect the Pi Zero to a computer and check if it is recognized as a USB storage device.
Network Adapter:
Verify network access by sharing the host computer's internet.
System Recovery:
Use tools like testdisk to test recovery functions.
Future Enhancements
Add logging for recovery operations.
Enable remote monitoring via a web interface.
Add additional USB gadget modes (e.g., HID for keyboard simulation).
Author
This project was developed by Ch312 C3uZ (also known as Christiano Cruz), an American-Brazilian cybersecurity expert, penetration testing specialist, and firmware modification professional. Ch312 C3uZ is passionate about building innovative tools using Raspberry Pi devices and creating solutions for system recovery and management.

For more information or to get in touch, visit:

GitHub: github.com/Ch312-C3uZ
Website: ch312c3uz.dev
License
This project is open-source and licensed under the MIT License. Contributions are welcome!



This version includes the author's name and background, making it more personal and professional. Let me know if you'd like further modifications!







