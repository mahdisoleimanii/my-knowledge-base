---
title: How to fully install Ubuntu 24.04 on a flash drive / USB stick (amd64 architecture)
description: A step-by-step guide for Ubuntu 24.04
icon: ubuntu
---

# How to fully install Ubuntu 24.04 on a flash drive / USB stick (amd64 architecture)

## Background

> You can skip this if you want to get straight to the instructions

So one day I decided I want to get hands-on experience with Linux, and so it got me thinking: How can I install Linux on my computer without messing up my current Windows installation? After weighing the options, I decided to install Ubuntu on a USB stick. I bought AN 128 GB USB 3.2 Gen 1 stick and got to work. I created an Ubuntu 22.04 live USB installer (for some reason I didn't go for the Ubuntu 24.04) using Rufus. I encountered a weird problem when I booted into my live Ubuntu 22.04 and chose "Try Ubuntu": The colors became a mix of yellow and green. I realized it has something to do with `gdm3` and it's an easy fix. Anyway, I created 4 partitions on the USB stick: 1 for EFI System partition, 1 for swap, an ext4 partition for root, and an ext4 partition for home. I remember vividly the frustration I felt when I realized the bootloader was installed on my internal SSD alongside my Windows boot files even though I specifically chose to install it on the USB stick. After all, this is what makes a USB installation portable, right? I fixed this issue with my live USB installer and mounted the boot files onto the USB stick manually. Now I had another problem: I tested writing speed of my USB stick and it was abysmally slow. I did some research (with AI Chatbots) and after rejecting the idea of buying an external SSD drive, I realized that the root partitions has to be formatted with F2FS (Flash-Friendly File System) in order to get decent speeds on flash storage. Now, for this you need a tool called `f2fs-tools` that for some reason is not included in the Ubuntu 22.04 live installer AND I couldn't install it using `apt` because it couldn't find the package (Yes, I ran `sudo apt update` first). So I made an Ubuntu 24.04 Installer instead (at the time of writing this, the latest LTS version is Ubuntu 24.04.3) and voila, I ran `sudo apt install f2fs-tools` and it worked! This time I created 3 partitions: 1 for EFI System partition, an ext4 partition for boot, and an F2FS partition for root (I decided to skip the home partition this time). I also managed to find the workaround to install the bootloader on the USB stick and not on my internal SSD. After installation, I tested the writing speeds and it was MUCH better (about 5x better)! And now I'm here to share the steps with you.

## System Specifications

- **PC**: Windows 11 installation on an internal SSD (UEFI mode)
- **Processor**: Intel Core i7
- **RAM**: 16 GB

---

**Category:** Linux  
**Last Updated:** 2025-11-26

## Instructions

### Step 1: Create a Live Ubuntu 24.04 USB Installer

Using Rufus:

1. Download the Ubuntu 24.04 ISO from the official website
2. Download and open Rufus (portable version is sufficient)
3. Select your USB stick in the "Device" dropdown
4. Click "SELECT" and choose the Ubuntu 24.04 ISO you downloaded
5. Set "Partition scheme" to "GPT" and "Target system" to "UEFI (non CSM)"
6. Click "START" and wait for it to finish (keep default settings in Rufus)
7. When prompted to choose a mode, select "Write in ISO Image mode (Recommended)"

### Step 2: Boot into Live Ubuntu 24.04

Boot into the live Ubuntu 24.04 from the USB installer you just created.

!!! note
    You don't need to change your boot order settings if your computer supports booting from USB natively. Just press the boot menu key (usually `F12`, `F10`, or `Esc`) during startup and select the USB drive.

**If you experience color distortion (yellow/green tint):**

1. Open a terminal and run:

   ```bash
   sudo nano /etc/gdm3/custom.conf
   ```

2. Uncomment the line `#WaylandEnable=false` by removing the `#`, change it to `WaylandEnable=true`, then save and exit (press `Ctrl + X`, then `Y`, then `Enter`)
3. Run:

   ```bash
   sudo systemctl restart gdm3
   ```

(source: [Ask Ubuntu](https://askubuntu.com/a/1408768))

### Step 3: Install f2fs-tools

Once in the live Ubuntu environment, open the terminal and install `f2fs-tools`:

```bash
sudo apt update
sudo apt install f2fs-tools
```

### Step 4: Open GParted

Open "GParted" (it should be pre-installed in the live environment).

### Step 5: Select Your USB Stick

In GParted, select your USB stick from the top-right dropdown menu. **Make sure you select the correct drive to avoid data loss on your internal drives.** Check for its size to confirm.

### Step 6: Delete Existing Partitions

If there are any existing partitions on the USB stick:

1. Right-click on each partition and select "Unmount" (if mounted)
2. Right-click again and select "Delete"
3. After deleting all partitions, click the green checkmark button to apply the changes

### Step 7: Create New Partitions

Create the following partitions on the USB stick:

1. **EFI System Partition**
   - Size: 512 MB
   - File system: FAT32
   - Label: USB-EFI

2. **Boot Partition**
   - Size: 1024 MB
   - File system: ext4
   - Label: USB-boot

3. **Root Partition**
   - Size: Remaining space
   - File system: f2fs
   - Label: USB-root

After creating the partitions, click the green checkmark button to apply the changes.

!!! note
    Right-click on the first partition you created (EFI System Partition) and select "Manage Flags". Check the boxes for "boot" and "esp", then click "Close".

### Step 8: Disable EFI Boot Flags on Internal Drive

Select your internal drive where you have your Windows installation. Right-click on its EFI System Partition (usually around 100-500 MB, FAT32), and select "Manage Flags". Uncheck the boxes for "boot" and "esp" (another flag may automatically get checked, don't worry about it), then click "Close".

!!! warning
    THIS STEP IS **CRUCIAL** to prevent the Ubuntu installer from installing the bootloader on your internal drive. We will re-enable these flags later.

### Step 9: Close GParted and Open Ubuntu Installer

Close GParted and open the "Install Ubuntu" application on the desktop.

### Step 10: Configure Installation Settings

Proceed with the installation:

- Select "Do not connect to the Internet" (optional but recommended)
- Keep the installation minimal (no third-party software)

### Step 11: Select Manual Installation

When you reach the "Installation type" screen, select "Manual Installation".

### Step 12: Identify and Assign Partitions

Find your USB stick (based on its size) which will show up in the form of `/dev/sdX` (where `X` is a letter). When installing from a live installer:

- Your internal HDD (if you have one) will be `/dev/sda`
- Your live installer will be `/dev/sdb`
- Your USB stick will be `/dev/sdc` (or another letter if you have more drives)

Assign the partitions as follows:

1. Select the USB-EFI System Partition (first partition), click "Change", set its mount point to `/boot/efi`, then click "OK"
2. Select the USB-boot Partition (second partition), click "Change", set its mount point to `/boot`, then click "OK"
3. Select the USB-root Partition (third partition), click "Change", set its mount point to `/`, then click "OK"

### Step 13: Set Bootloader Installation Device

At the bottom of the window, there is a dropdown menu for "Device for bootloader installation". Make sure to select your USB stick in the form of `/dev/sdX` (where `X` is a letter corresponding to your USB stick).

!!! danger
    DO NOT select any partition (like `/dev/sdc1`), just select the drive itself (like `/dev/sdc`).

### Step 14: Complete Installation

Proceed with the installation. Once it's done, **DO NOT restart your computer yet**. Instead, select "Continue Testing" to go back to the live Ubuntu environment.

### Step 15: Re-enable Internal Drive Boot Flags

Open "GParted" again, select your internal drive, right-click on its EFI System Partition, and select "Manage Flags". Re-check the boxes for "boot" and "esp". Click "Close".

### Step 16: Optimize Mount Options (Optional but Recommended)

1. Open a terminal
2. Mount the new installation to edit its settings:

   ```bash
   sudo mount /dev/sdX3 /mnt  # Replace sdX3 with your USB-root partition
   sudo nano /mnt/etc/fstab
   ```

3. In the `fstab` file, look for the line that corresponds to your **Root Partition** (`/`)
4. Change the mount options from `defaults` to `defaults,noatime,nodiratime`
5. Save and exit (press `Ctrl + X`, then `Y`, then `Enter`)
6. Unmount the partition:

   ```bash
   sudo umount /mnt
   ```

### Step 17: Restart Computer

Now you can restart your computer. When prompted, remove the live USB installer and press Enter, but keep the USB stick with the full Ubuntu installation plugged in.

### Step 18: Boot from USB Installation

Your computer should now boot into the Ubuntu installation on your USB stick. If you want to use it on another computer, just plug in the USB stick and select it from the boot menu during startup.
