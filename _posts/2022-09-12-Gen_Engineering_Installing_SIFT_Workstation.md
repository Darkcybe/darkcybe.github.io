---
title: Building SIFT Workstation on Ubuntu 20.04 LTS
categories: [General IT and Security,Engineering]
tags: [SIFT,]
comments: true
---
The good folks at SANS Institute have put together and maintain a pre-configured collection of tools to assist DFIR analysts in their war against the cyber baddies. If you’ve taken one of SANS DFIR training courses, you’re likely familiar with SIFT. For those that haven’t and would like to test it out, SIFT contains some great open-source tools to support many forensic tasks.

SANS do offer a preconfigured VM ready for download at this link, [SIFT Workstation Download](https://www.sans.org/tools/sift-workstation/). However this version is somewhat behind the times, my preferred method is to create a template SIFT machine based off of a fresh Ubuntu VM, and I also run SIFT on my WIN11 machine on server mode through Ubuntu WSL, but that’s for another day.

Alas, after a very brief introduction in assuming the reader, you, already knows what the SIFT environment has to offer. Lets get to the process of creating an Ubuntu VM and installing the thing!

# Installing Ubuntu 20.04 LTS
The current SIFT version is only supported by Ubuntu 20.04 Desktop/Server editions with this procedure being carried out on the latest distribution available from [Ubuntu](https://ubuntu.com/download/desktop).

1. Following the wizard setup for the hypervisor software of your choosing (I use and prefer VMware Workstation Pro 16 – Because dark mode!)
- Configuration and customization is fairly straight forward at this point, the SIFT install takes up roughly 60GB worth of precious HDD space so ensure you allocate enough storage when setting up the VM. My recommendation is to increase Memory to at least 16GB (for processing the timelines and memory things), more processors (much brain), remove the printer and sound card. Circling back to storage, depending on how you run investigations, you can assign a maximum disk size that can hold investigation data (1TB???) or, as i do, simply mount and evidence drive and utilize an external SSD to store case files.
2. The next part is super fun! You can grab a beer (or coffee depending on what time of day it is) and watch as the Ubuntu installer does it thing!
3. Once the beer is finished, grab another one and then await the completion of the install. Finally once it is complete and you’re presented with the desktop view, the very first thing we must do is remove the help icon from the favorites menu, because we simply don’t need it… We’re adults.
4. Now, as the good security conscious people that we are I will safely assume that you know what to first do when installing a new OS (if not, it’s updates). If you’re spending the time to read this, there is a fair chance that the software updater has already prompted you, If not, or you’re new to the Linux Terminal’ator just copy and paste this after pressing CTRL+ALT+T and once done, remove from the oven and restart the boi for good measure.
    ```bash
    sudo apt-get update && sudo apt-get upgrade && sudo dist-upgrade
    sudo autoremove
    ```
    {: .nolineno }
    
# Installing SIFT-CLI
1. For those still with me, we know need to install the SIFT CLI package in order to install and maintain our SIFT environment. So head on onto the [teamdfir](https://github.com/teamdfir/sift-cli#installation) github page and download the latest release files.
2. Install [cosign](https://github.com/sigstore/cosign/releases/latest) and Validate the signature 
   ```bash
   cosign verify-blob --key sift-cli.pub --signature sift-cli-linux.sig sift-cli-linux
   ```
   {: .nolineno }
3. Move the file to `/usr/local/bin/sift` and change the permissions of this little fella to ensure it runs to the best of his ability
   ```bash
   sudo mv sift-cli-linux /usr/local/bin/sift && chmod 755 /usr/local/bin/sift
   ```
   {: .nolineno }

# Installing SIFT via SIFT-CLI
1. Alright, now the sift-cli should be ready to go. in good practice type `sift –help` to see the manual. And proceed to the next beer because we’ve got another little while of waiting whilst the SIFT package installs. Have you got your beer ready? Ok, now the last command is the package install `sudo sift install`. Once complete, make a snapshot and you’re good to track down those pesky hack0rz.