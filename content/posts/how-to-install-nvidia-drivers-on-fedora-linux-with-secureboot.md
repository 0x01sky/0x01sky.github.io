+++
date = '2026-05-16T17:59:32+01:00'
draft = false
title = 'How to Install Nvidia Drivers on Fedora Linux With Secureboot'
+++

 This guide would provide instructions on installing the nvidia drivers on any fedora desktop or wm, if you're someone struggling to get your nvidia drivers working with secureboot, this is the right place for you !

# Note
- This guide is based on the official RPM Fusion docs, if you're using Fedora Atomic Desktops, you may need to refer to the rpm's fusion full documentation :
 
 ```bash
 https://rpmfusion.org/Howto/NVIDIA
 https://rpmfusion.org/Howto/Secure%20Boot

 ```
# Requirements
- Fedora 40+
- Secure boot must be enabled in setup mode in your BIOS/UEFI sttings (often called "Custom mode" in some BIOS versions) before proceeding !
- You must remove any existing Nvidia drivers before proceeding by typing :

```bash
 dnf rm xorg-x11-drv-nvidia\*

```
- After removing the drivers, you must reboot!

# Optional Step (Debugging Step)

for easier debugging to spot errors, you can remove 'quiet' and 'rhgb' boot arguments:

```bash
# Disables 'quiet' mode & removes red hat's graphical boot
sudo grubby --update-kernel=ALL --remove-args="quiet rhgb"

# Enables 'quiet' mode & adds red hat's graphical boot
sudo grubby --update-kernel=ALL --args="quiet rhgb"
```

# Steps

- Add RPM Fusion Third Party repositories:

```bash
sudo dnf in https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf in https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

```
- Update the system :

```bash
sudo dnf up --refresh

```
- Install signing modules :

```bash
sudo dnf in kmodtool akmods mokutil openssl

```
- Generate a key :

```bash
sudo kmodgenca -a 

```
- Import your recently created a key (you may be prompted for password, it doesn't need to be long) :

```bash
sudo mokutil --import /etc/pki/akmods/certs/public_key.drivers

 ```
- Reboot

```bash
reboot

```

- As soon as you reboot , MOK manager would ask if you want to proceed with booting or enrolling the key.
- In this case , you would choose "Enroll MOK" -> "Continue" and enter your password that was created in step 5 (it follows the qwerty layout).

![MOK Picture](/images/mok.png)
![MOK 2 Picture](/images/mok-1.png)

- Install Nvidia Drivers

```bash
sudo dnf install gcc kernel-headers kernel-devel akmod-nvidia \
 xorg-x11-drv-nvidia xorg-x11-drv-nvidia-libs.{i686,x86_64} \
 libva-nvidia-driver.{i686,x86_64} xorg-x11-drv-nvidia-cuda

```

- Wait a few minutes for the modules to build, then check if they are built using:

```bash
modinfo -F version nvidia

```
![Reference](/images/nvidia.png)

- if the output was `ERROR: Module nvidia not found`, the modules are still building!

- Ensure the modules are built for the currently running kernel and boot image:

```bash
sudo akmods --force  && sudo dracut --force
```

- Reboot, and everything is done !

![Preview](/images/preview_nvidia.png)

## Credits to : @roworu
