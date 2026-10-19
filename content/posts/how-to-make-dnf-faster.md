+++
date = '2026-10-19T22:03:26Z'
draft = false
title = 'How to Make Dnf Faster'
+++

# Introduction :

- In this guide, we will be covering on how to make dnf faster which is the package manager used in RHEL based linux distributions such as CentOS Stream, Fedora Linux, Rocky Linux, Alma Linux, etc

# Getting Started :

- This guide is intended to make dnf faster by using 2 ways, so make sure to check which version of dnf you have first by running :

```bash
dnf --version | head -1
```

- If your dnf version is dnf4 , you may will need to go to `/etc/dnf/dnf.conf` with any editor like nano or vim, to add `max_parallel_downloads=15` and `fastestmirror=True`, and saving with `CTRL+O`, and exiting with `CTRL+X` in nano, whereas in vim , with `wq`

- You can use a one liner for this using tee :

```bash
echo -e "fastestmirror=True\nmax_parallel_downloads=15" | sudo tee -a /etc/dnf/dnf.conf &>/dev/null
```

- For dnf5, you can use `config-manager` option to set the options for dnf, and to do that we need to specify `setopt` and then specify the options with either boolean values or integer values, check if `dnf5-plugins` are installed and then run :

```bash
sudo dnf config-manager setopt fastestmirror=True max_parallel_downloads=15
```

- So what `fastestmirror=<boolean>` option do? , if you know Fedora and other EL flavors by default has this option set as `False`, which makes the installation of packages slower, so by enabling the `fastestmiror`, You get the fastest mirrors that are geographically closer to you !

- What about `max_parallel_downloads=<int>` ?, with this option dnf can fetch multiple packages at the same time, in our case it's 15 which is a massive amount and really great for fast wifi !

# TLDR

- While this may not be a guarranteed fix for slower wifis, it's highly recommended to set `max_parallel_downloads` at `10`, to avoid having timeouts by mirrors.

**Best Wishes !**

