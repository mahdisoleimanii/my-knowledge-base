---
title: How (not) to install Lineage OS on your Android device
description: A step-by-step guide to (not) installing Lineage OS on your Android device.
icon: android
---

# 📱 How (not) to install Lineage OS on your Android device

> ⚠️ This is the story of a bitter experience.

Let's address the elephant in the room: You WILL most likely regret doing this, after you have done it. Installing custom ROMs like Lineage OS comes with significant challenges, out of which I will highlight three major ones:

- 🔓 **Voiding warranty**: If you are reading this article, it probably means you have an old device that is out of warranty anyway. However, the reason is that you have to unlock the bootloader, which voids the warranty.

- 🏦 **Banking apps**: Many (almost all) banking apps refuse to run on rooted or custom ROM devices due to security concerns. After installing Lineage OS, you will get to know something called **Integrity Checks**. I will explain this more.

- 📉 **Losing Device Features**: Custom ROMs often lack support for certain hardware features or proprietary software components, leading to a degraded user experience.

---

Now that we addressed the challenges, let me tell you about the most important reason why you should NOT install Lineage OS: **Integrity Checks**.

## 🛡️ Device Integrity Checks

Device Integrity Checks are security mechanisms implemented by app developers to ensure that their applications are running on a secure and unmodified environment. When you install a custom ROM like Lineage OS, you are essentially modifying the underlying system, which can trigger these checks and cause apps to refuse to run or behave unexpectedly.

### 3 Levels of Device Integrity Checks

- 🟢 **Basic integrity**: This is the most basic level of integrity check. It checks if the device is rooted or has a custom ROM installed. If it detects any modifications, it may refuse to run the app. You can most likely bypass this check without much trouble.

- 🟠 **Device integrity**: This level of integrity check goes a step further. Bypassing this check is more difficult, but still possible with some effort.

- 🔴 **Strong integrity**: There are of course methods to bypass this check, but they are complex and may not work for all apps. This level of integrity check, is hardward-based so it is almost impossible to bypass it completely, due to how Google tries to fight against custom ROMs and rooting.

## 💾 Persist image

The persist image is a partition on your Android device that stores calibration data for various hardware components, such as the camera, sensors, and radio. When you install a custom ROM like Lineage OS, the persist image may get corrupted or overwritten, leading to issues with these hardware components.

### 🔐 Why Back Up the Persist Image?

This is why, before installing any custom ROM (if you still want to), it is CRUICIAL to **back up the persist image** and restore it BEFORE flashing your original stock ROM after you realized you made a mistake. 

Failing to do so may result in malfunctioning hardware features, such as:
- 📷 Poor camera quality
- 📡 Inaccurate sensor readings
- 📶 Connectivity problems
- ⚡ And MANY MORE

## 📦 Original Stock ROM

Before installing Lineage OS, it is essential to **download the original stock ROM** for your specific device model. This is because, after installing Lineage OS, you may encounter issues that require you to revert back to the stock ROM. 

⚠️ **Important:** You can visit the manufacturer's website to get your original stock ROM, BUT **you usually have to pay for it**. 

## 📖 My Experience

I installed Lineage OS on my old Mi 9T (Redmi K20) device, which I use as my primary phone. After reading the previous sentence, you can probably conclude that I made a mistake. If it was not my primary phone, I may have never written this article.

### 🏧 The Banking App Problem

The most important app on my phone is my banking app. I tried HARD and I mean HARD to get the banking app to work but I failed, there is no other word for it, miserably.

### 🔥 The Bootloader Lock Disaster

After realizing that I made a mistake, I tried to revert back to the stock ROM, BUT, despite having backed up my persist image, I forgot to restore it BEFORE flashing the stock ROM and locked my bootloader without flashing the persist image. And there the problems arose. 

Xiaomi being Xiaomi and my country being my country, it took me an arduous 48-hours to be able to unlock the bootloader again. After finally being able to unlock the bootloader, I flashed the persist image and the stock ROM and finally I got back to the point in time before I installed Lineage OS 🙂. 