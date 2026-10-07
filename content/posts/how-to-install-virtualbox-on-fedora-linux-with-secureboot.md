+++
date = '2026-10-07T20:00:03Z'
draft = true
title = 'How to Install Virtualbox on Fedora Linux With Secureboot'
+++

In This guide, we would provide similar instructions provided for nvidia drivers, if you're someone struggling to get virtual box kernel modules to get signed , and looking for a geniune and "easy to follow" guide , this is the rightest place for you !

# Note
- This guide is based on official RPM Fusion docs, if you consider using Fedora Atomic Desktops, you may need to refer to the rpm's fusion full documentation :

```bash
https://rpmfusion.org/Howto/Secure%20Boot
https://rpmfusion.org/Howto/VirtualBox

```

# Requirements
- Fedora 40+
- Secure boot must be enabled in setup mode in your BIOS/UEFI settings (often called "Custom mode" in some BIOS versions) before proceeding !
- You must remove any existing VirtualBox and it devels before proceeding bytyping:

```bash
 sudo dnf rm VirtualBox VirtualBox-{akmod,devel} -y
```

- After VirtualBox removal , you need to have the latest kernel ,by checking for any update and installing any available one specifically to get the latest available kernel on fedora :

**Note:** Avoid installing custom kernels like cachyos-kernels or zen-kernels , this is not tested and could result in a failure !

```bash
sudo dnf up -y

```

# Steps

- Add RPM Fusion Free Repository:

```bash
sudo dnf in https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

```
- Updating the repository :

```bash
sudo dnf check-update

```
- Install mok utilities for VirtualBox kernel modules signing :

```bash
sudo dnf in kmodtool akmods mokutil openssl -y

```

- Now, we need to use ```kmodgenca``` to generate the signing keys :

```bash
sudo kmodgenca -a # if its already there to overwrite it, pass the -f flag to force it

```
- Import your recently created signing keys (you may be prompted for a password, it doesn't need to be cryptographically long) :

```bash
sudo mokutil --import /etc/pki/akmods/certs/public_key.drivers

```
- Now you can reboot

```bash
reboot # or systemctl reboot

```

- As soon as you reboot, MOK manager would ask you weither you want to enroll your keys or continue booting.

- In this case, you would choose "Enroll MOK" to enroll your mok signing key and then "Continue" and then you can enter your previously typed password when you imported the public key (it uses qwerty layout).

![MOK Picture](/images/mok.png)
![NOK 2 Picture](/images/mok-1.png)

- Now we can go ahead and install VirtualBox:

```bash
sudo dnf in gcc kernel-{headers,devel,devel-matched} \
 VirtualBox akmod-VirtualBox VirtualBox-guest-additions

```

- To ensure the modules are built for your current latest kernel, run :

```bash
sudo akmods --force && sudo dracut --force

```

- To ensure that the kernel module is available :

```bash
sudo modinfo -F version vboxdrv

```

- Do a final reboot !

```bash
 reboot

```

- Make sure the modules are loaded using :

```bash
sudo modprobe vboxdrv vboxnetadp vboxnetflt

```

