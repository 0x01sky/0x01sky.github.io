+++
date = '2026-10-22T20:41:20Z'
draft = false
title = 'Converting a Deb Package to an Rpm Package'
+++

# Introduction

- In this guide, we would be providing guides on how to convert a Debian debpackage to a Red Hat rpm package, we will use alien which is a tool intended for this purpose, furthermore we would use rpmbuild to build the package on any architecture and using the generated .spec filewith alien.

## Pre-requisites

- rpmdevtools
- alien
* You can install it with:

```bash
sudo dnf in rpmdevtools alien -y
```
# Getting Started

- First, we gonna prepare the Debian deb package, to convert it to a rpm package , we will use alien :

![Part1](/images/deb-to-rpm-part-1.png)

- recapitulating on what `alien -r -g -v obsidian-1.14.4_amd64.deb` - `-g` generates a temp directory to store the build
 - `-r` convert to rpm packages.
 - `-v` for verbose output

- Now, we can build the package using `rpmbuild` with the specified package .spec file, you may wait a few mins for the package to be built, by running :

```bash
sudo rpmbuild --target=$(uname -m) --buildroot $PWD -bb <package_name.spec>
```

![Part2](/images/deb-to-rpm-part-3.png)

- After the package build, you will have the rpm package :

![Part4](/images/deb-to-rpm-part-4.png)

- if you wish to install it, you can simply use :
    - with rpm
    ```bash
    sudo rpm -ivh <package.rpm>
    ``` 
    - with dnf
    ```bash
    sudo dnf in <package.rpm>
    ```
**Best wishes !**
