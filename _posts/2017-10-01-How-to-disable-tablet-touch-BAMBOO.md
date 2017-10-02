Hello folks, good morning

Today I'll show you how to disable Bamboo tablet touch, with this feature enable you will be able to control the cursos using your finger, this is interesting BUT when you need to any artistic work will be painful, then let's do it.

{% include figure image_path="/assets/images/back.jpg" %}

First, let's check your environment and what is the equipment that you have

```
[root@dvader ~]# lsusb | grep -i wacom
Bus 002 Device 013: ID 056a:00de Wacom Co., Ltd CTH-470 [Bamboo Fun Pen & Touch]
[root@dvader ~]#
```

xsetwacom is the application that will help you a lot

```
[root@dvader ~]# xsetwacom 
Usage: xsetwacom [options] [command [arguments...]]
Options:
 -h, --help                 - usage
 -v, --verbose              - verbose output
 -V, --version              - version info
 -d, --display "display"    - override default display
 -s, --shell                - generate shell commands for 'get'
 -x, --xconf                - generate xorg.conf lines for 'get'

Commands:
 --list devices             - display detected devices
 --list parameters          - display supported parameters
 --list modifiers           - display supported modifier and specific keys for keystrokes
 --set "device name" parameter [values...] - set device parameter by name
 --get "device name" parameter [param...]  - get current device parameter(s) value by name
[root@dvader ~]#
```

Below let's list all devices connected

```
[wpinheir@dvader ~]$ xsetwacom --list devices
Wacom Bamboo 16FG 4x5 Finger touch	id: 15	type: TOUCH     
Wacom Bamboo 16FG 4x5 Finger pad	id: 16	type: PAD       
Wacom Bamboo 16FG 4x5 Pen stylus	id: 17	type: STYLUS    
Wacom Bamboo 16FG 4x5 Pen eraser	id: 18	type: ERASER    
[wpinheir@dvader ~]$
```

Below we can see the parameter that we need update

```
[root@dvader ~]# xsetwacom --list parameters | grep ^Touch
Touch            - Turns on/off Touch events (default is on). 
[root@dvader ~]#
```

Bellow command to disable Touch feature, then after that feel free to use your pen without issues on Linux.
```
# xsetwacom --set "Wacom Bamboo 16FG 4x5 Finger touch" Touch off
```

And with command below you will be able to check what is the actual configuration

```
[wpinheir@dvader ~]$ xsetwacom --get "Wacom Bamboo 16FG 4x5 Finger touch" Touch 
off
[wpinheir@dvader ~]$
```

NOTE: This command will be lost if you restart the computer, the to keep permanent I'll show 2 ways.

1. Add command above your your local script just to be executed everytime your machine start

or

2. Check steps below
  - We got our tablet model with command *xsetwacom --list devices*
  - rpm -qa | grep wacom to see Wacom packages related
    ```
    [root@dvader ~]# rpm -qa | grep wacom
    xorg-x11-drv-wacom-0.29.0-1.el7.x86_64
    libwacom-0.12-1.el7.x86_64
    libwacom-data-0.12-1.el7.noarch
    [root@dvader ~]#
    ```
  - Checking on data package, let's looking for information about bamboo and tablets
    ```
    [root@dvader ~]# rpm -ql libwacom-data-0.12-1.el7.noarch | grep bam | grep tablet
    /usr/share/libwacom/bamboo-16fg-4x5.tablet
    /usr/share/libwacom/bamboo-2fg-4x5.tablet
    /usr/share/libwacom/bamboo-2fg-6x8.tablet
    /usr/share/libwacom/bamboo-2fg.tablet
    /usr/share/libwacom/bamboo-craft.tablet
    /usr/share/libwacom/bamboo-one.tablet
    /usr/share/libwacom/bamboo-pen.tablet
    [root@dvader ~]#
    ```
  - In our case the model is 16fg, then let's update the file */usr/share/libwacom/bamboo-16fg-4x5.tablet*
  - Inside this file, change the parameter Touch from true to false, then restart the computer and the new configuration will be ok.
    ```
    Touch=true
    to
    Touch=false
    ```


So hope this trick help you in your environment, after do this feel free to use your tablet with Gimp or InkScape.

Have a good one.
Waldirio
