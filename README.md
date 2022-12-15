# Operating-System-Project-1
Develop a custom Operating System from the open source Yocto project.

In this project we built a custom image for our OS (Operating Sytem) from the Yocto Project and added custom packages based on our requirement before building the image. Finally tested the packages on the emulator provided by the Yocto project.

## What is Yocto Project?
Yocto Project is a cool open-source community project that allows you to build your own Linux system from scratch to booting into your embedded hardware, which makes embedded Linux simple and accessible. It helps developers create customized systems based on the Linux kernel by providing useful templates, tools, and methods, and wide hardware architectures support on ARM, PPC, MIPS, x86 (32 & 64 bit). Whereas other Linux distributions are built for enterprise servers and workstations and then (possibly) tailored down for embedded use cases, the Yocto Project enables the build of customized distributions for embedded devices. In a disparate market with heterogeneous requirements, the project seeks to define a common ground for embedded development, independent of the underlying architecture of the hardware.
## Prerequisites
1. 50 GB of free disk space
2. Runs a supported Linux distribution (i.e. recent releases of Fedora, openSUSE, CentOS, Debian, or Ubuntu). For a list of Linux distributions that support the Yocto Project, see the Supported Linux Distributions section in the Yocto Project Reference Manual. For detailed information on preparing your build host, see the Preparing the Build Host section in the Yocto Project Development Tasks Manual.
3. 
    Git 1.8.3.1 or greater
    
    tar 1.28 or greater
    
    Python 3.5.0 or greater.
    
    gcc 5.0 or greater.
    
4. If you are using Ubuntu, use Ubuntu 20.04 LTS. Ubuntu 22 right now does not have pylint3 which gives error while building the package.
## Steps for building the image

### Build Host packages
You must install essential host packages on your build host. The following command installs the host packages based on an Ubuntu distribution:

```$ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint3 xterm python3-subunit mesa-common-dev```

> Note For host package requirements on all supported Linux distributions, see the [Required Packages for the Build Host](https://docs.yoctoproject.org/3.1.21/ref-manual/ref-system-requirements.html#required-packages-for-the-build-host) section in the Yocto Project Reference Manual.

### Clone the poky repository in your destination folder
```
touseef@touseef-2001:~/OS_PROJECT1$ git clone git://git.yoctoproject.org/poky
Cloning into 'poky'...
remote: Enumerating objects: 584802, done.
remote: Counting objects: 100% (3438/3438), done.
remote: Compressing objects: 100% (851/851), done.
remote: Total 584802 (delta 2846), reused 2890 (delta 2568), pack-reused 581364
Receiving objects: 100% (584802/584802), 188.52 MiB | 776.00 KiB/s, done.
Resolving deltas: 100% (424826/424826), done.
```
> Note: You can use script command to log everything into a file for tracking purposes. ``` script logFile``` Here all the log will be stored in the logFile for that particular terminal session. 

Move to the poky directory and take a look at the tags:
```
$ cd poky
$ git fetch --tags
$ git tag
```

I have selected dunfell-23.0.5 branch.

```
touseef@touseef-2001:~/OS_PROJECT1/poky$ git checkout tags/dunfell-23.0.5 -b dunfell
Switched to a new branch 'dunfell'
```

1. **Initialize the Build Environment:** From within the poky directory, run the oe-init-build-env environment setup script to define Yocto Project’s build environment on your build host.
```
touseef@touseef-2001:~/OS_PROJECT1/poky$ source oe-init-build-env
You had no conf/local.conf file. This configuration file has therefore been
created for you with some default values. You may wish to edit it to, for
example, select a different MACHINE (target hardware). See conf/local.conf
for more information as common configuration options are commented.

You had no conf/bblayers.conf file. This configuration file has therefore been
created for you with some default values. To add additional metadata layers
into your configuration please add entries to conf/bblayers.conf.

The Yocto Project has extensive documentation about OE including a reference
manual which can be found at:
    http://yoctoproject.org/documentation

For more information about OpenEmbedded see their website:
    http://www.openembedded.org/


### Shell environment set up for builds. ###

You can now run 'bitbake <target>'

Common targets are:
    core-image-minimal
    core-image-sato
    meta-toolchain
    meta-ide-support

You can also run generated qemu images with a command like 'runqemu qemux86'

Other commonly useful commands are:
 - 'devtool' and 'recipetool' handle common recipe tasks
 - 'bitbake-layers' handles common layer tasks
 - 'oe-pkgdata-util' handles common target package tasks
```
> Everytime you open a new terminal to use the bitbake command you have to initialize the environment.
2. **To install the custom packages:** Inside the ```touseef@touseef-2001:~/OS_PROJECT1/poky/build$ ``` directory go to conf folder and open local.conf.
Add 
``` 
IMAGE_INSTALL_append = " file sudo dpkg apt packagegroup-core-buildessential"
``` 
to the local.conf file to add the following packages ```file, sudo, dpkg, apt, packagegroup-core-buildessential``` to your custom OS build.
> To get the list of all the supported packages to include, run the command ``` touseef@touseef-2001:~/OS_PROJECT1/poky/build$ bitbake-layers show-recipes```

> Make sure to add the space before the first file package, here just before the package name "file".
> Inside the local.conf file you will see that the target machine is set to qemux86-64 ```MACHINE ??= "qemux86-64"``` ,this is the hardware for which the custom OS will be build. Here we are building to test in the emulator i.e. qemu provided by Yocto.

Run the command ```touseef@touseef-2001:~/OS_PROJECT1/poky/build$ bitbake core-image-minimal``` , you can use core-image-sato or any other as per your requirement. Image building will start after this, you need an internet connection. This may take some time based on your system performance.

### 3. Final step: 
Run the image in emulator to test the image built. Run the command ```touseef@touseef-2001:~/os-qemu/poky/build$ runqemu```.
![image](https://user-images.githubusercontent.com/79767012/207830809-ff3b1781-3dfb-44e5-8065-effe690e791b.png)
>Password is : root. 

Now you can check the packages that you configured are installed or not.
