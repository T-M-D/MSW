# Deploy MSW
The initial purpose of MSW is to replace the VMware vMA appliance that VMware depreciated.
* This was basically SUSE 11 with the vSphere Perl SDK installed on it
* The box293_check_vmware plugin used the vMA to run from as it was nice and easy to get running with minimal fuss
* Since it has been depreciated the newer SDK will not install on it without a lot of effort
  * Specifically the 6.7 SDK is required to communicate with vSphere / vCenter 6.7
  * The older SDK on the vMA does not work

The steps here basically replace the "Deploy vMA Appliance" in the box293_check_vmware manual. After following these steps you would resume the manual at the "Transfer the box293_check_vmware plugin to the vMA" section.
From this point on, MSW refers to the machine you are using as the "vMA".

These steps should work on RHEL / CentOS / Oracle Linux version 7.x. Other operating systems (OS) will be added later, but for now this just gets it working.

# Deploy Operating System
Perform a basical install of RHEL / CentOS / Oracle Linux version 7.x. You do not require the desktop GUI, all the steps in this guide will be performed from a terminal session.
Once you have installed the OS you should configure it with a static IP address before continuing.

# Download vSphere Perl SDK
You will need to visit the VMware website to download the vSphere Perl SDK, it's not possible to provide a download URL in this guide as you require a user account to perform the download.
You can download it from:
* https://code.vmware.com/web/sdk/67/vsphere-perl
* You need the 64 bit version, for example:
  * `VMware-vSphere-Perl-SDK-6.7.0-8156551.x86_64.tar.gz`

Once you've downloaded the SDK you will need to transfer it to your MSW to the /tmp/ directory.

# Install Pre-requisites
Login to your MSW as the root user and execute the following command to install the pre-requisite software:
```bash
yum install -y gcc openssl-devel cpan libxml2-devel xml2 libuuid-devel openssh-clients
```

Some Perl modules also need to be installed:
```bash
PERL_MM_USE_DEFAULT=1
cpan -i UUID
cpan -i Date::Format
```

# Install vSphere Perl SDK
Execute the following commands to install the vSphere Perl SDK:
```bash
cd /tmp
tar xzf VMware-vSphere-Perl-SDK-6.7.0-8156551.x86_64.tar.gz
cd vmware-vsphere-cli-distrib/
./vmware-install.pl EULA_AGREED=yes
```

You will be prompted to answer some questions, simple answer `yes` when prompted to continue.

A successfull install will produce this final message
```
The installation of vSphere CLI 6.7.0 build-8156551 for Linux completed 
successfully. You can decide to remove this software from your system at any 
time by invoking the following command: 
"/usr/bin/vmware-uninstall-vSphere-CLI.pl".
```

You may also receive a message stating that some Perl modules may be too old to work, these can be ignored.

# Create vi-admin User Account
The vMA appliance came with a user account called `vi-admin`. This is heavily referenced in the box293_check_vmware manual so for simplicity reasons you need to create that user account on the MSW so you can continue following the manual.
Create the user with this command:
```
useradd vi-admin
```
Define a password for the user with this command:
```
passwd vi-admin
```

# End
This finished the steps required to get the MSW deployed.
