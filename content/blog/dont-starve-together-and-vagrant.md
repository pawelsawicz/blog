+++
date = "2015-07-03T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Don't Starve Together and Vagrant"
weight = 20
aliases = [
    "/dont-starve-together-and-vagrant"
]
+++

219740_screenshots_2013-03-13_00004

I had few approaches into Vagrant but every time I was recreating what somebody have done already which was quite frustrating because I couldn't share what I have done and achieved. This time I came across with one problem that can be solved by using Vagrant and it's a field where it wasn't used before so it was also good time for me to do everything by my own.

First thing first, what is Vagrant and why I used it with Azure for Don't Starve Together Server (aka DST). Vagrant is quite old tool, first time I've heard about this, it was at Paul's session probably BuildStuff 2013. Basically this tool allows you to create configuration of your VM as a code, so it can be replicable on every VM provider or even at Cloud providers such as azure, aws. I chose vagrant and azure because I wanted to do easy and one command way to spin up don't starve together server on cloud.

Vagrant-dontstarve, repository with complete tutorial how to get started, most difficult part is certification, yea a know...MS screwed it up totally...I hope so that one day it will be simplified.

Basically repository contain two main files, Vagrantfile and script.sh.

Vagrantfile is a file where it keeps whole configuration about your : VM image, provider, provisioner, network and more. Here is a configuration for azure, additionally provisioning is global and can be applied to others providers.

Vagrant.configure('2') do |config|
    config.vm.box = 'azure'
    config.vm.provider :azure do |azure, override|
        override.vm.synced_folder ".", "/vagrant", disabled: true
        azure.mgmt_certificate = 'Path PEM Certificate from step 2.2 (To Fill out)'
        azure.mgmt_endpoint = 'https://management.core.windows.net'
        azure.subscription_id = 'Azure Subscription Id (To Fill out)'

        azure.vm_image = 'b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140416.1-en-us-30GB'
        azure.vm_user = 'VM User (To Fill out)' # defaults to 'vagrant' if not provided
        azure.vm_password = 'VM User Password (To Fill out)' # min 8 characters. should contain a lower case letter, an uppercase letter, a number and a special character

        azure.vm_name = 'VM Name (To Fill out)' # max 15 characters. contains letters, number and hyphens. can start with letters and can end with letters and numbers
        azure.cloud_service_name = '' # same as vm_name. leave blank to auto-generate
        # azure.deployment_name = '' # defaults to cloud_service_name
        azure.vm_location = 'West Europe' # e.g., West US
      #azure.private_key_file = 'C:\Users\PSawicz\myPrivateKey'
      azure.certificate_file = 'SSL PFX Certificate Generated 2.1 (To Fill out)'

      # Provide the following values if creating a *Nix VM
      azure.ssh_port = '22'
      azure.tcp_endpoints = '3389:53389' # opens the Remote Desktop internal port that listens on public port 53389. Without this, you cannot RDP to a Windows VM.
    end

    config.ssh.username = 'VM User (To Fill out)' # the one used to create the VM
    config.ssh.password = 'VM User Password (To Fill out)' # the one used to create the VM
    config.vm.provision "file", source: "./configs/server_token.txt", destination: "/home/azureuser/.klei/DoNotStarveTogether/server_token.txt"
    config.vm.provision "file", source: "./configs/settings.ini", destination: "/home/azureuser/.klei/DoNotStarveTogether/settings.ini"
    config.vm.provision "shell", path: "script.sh"
end


In the other hand script.sh is a simple bash script that contain instruction for a provisioning step, at this step you are trying to get your machine to certain state, it can set up privileges for users, install applications, configure them. At fallowing piece of code it tries to add architecture, then install all requires libs for SteamCMD.

echo "Starting provisioning"
echo "Setting up i386 architecture"
sudo dpkg --add-architecture i386 -y
echo "i386 architecture installed"
echo "Installing required packages"
sudo apt-get update -y
if [ $(dpkg-query -W -f='${Status}' lib32gcc1 2>/dev/null | grep -c "ok installed") -eq 0 ];
then
  sudo apt-get install lib32gcc1 -y
fi
if [ $(dpkg-query -W -f='${Status}' lib32stdc++6 2>/dev/null | grep -c "ok installed") -eq 0 ];
then
  sudo apt-get install lib32stdc++6 -y 
fi
if [ $(dpkg-query -W -f='${Status}' libcurl4-gnutls-dev:i386 2>/dev/null | grep -c "ok installed") -eq 0 ];
then
  sudo apt-get install libcurl4-gnutls-dev:i386 -y
fi


Right now it's configured only for one provider - Azure, but in few next days I would like to cover VirtualBox and AWS. There one issue that cannot be automated yet, DST requires to open 10999 UDP port, which now unfortunately vagrant-azure doesn't support, but I hope so that I will make PR soon, I do have to find some time and get ruby environment to work ;)

Happy playing!

Pawel