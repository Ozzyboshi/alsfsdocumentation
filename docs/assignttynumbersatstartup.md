# How to assign TTY numbers at startup
If you have multiple Amigas each one connected with its own serial USB Adapter, there are chances that the device files under /dev/ changes at each reboot.  
In my case, for example, I have 4 serial usb adapters and by default they are numbered gradually:

    pi@raspberrypi ~ $ ls -ls /dev/ttyUSB*
    0 crw-rw---T 1 root dialout 188, 0 Jan  1  1970 /dev/ttyUSB0
    0 crw-rw---T 1 root dialout 188, 1 Feb  3 19:58 /dev/ttyUSB1
    0 crw-rw---T 1 root dialout 188, 2 Jan  1  1970 /dev/ttyUSB2
    0 crw-rw---T 1 root dialout 188, 3 Jan  1  1970 /dev/ttyUSB3


The main problem here is that the numbers are not preserved after each reboot so you end up going by exclusion to find the corret Amiga you are trying to connect to.  
The best way to solve it is to assign logic names for each devfile and refer only to logic files.

To assign a logic file just find out PATH_ID and SUBSYSTEM for each devfile using udevadm, in the following example I will do this for /dev/ttyUSB1 :

```
udevadm info --query=property --name /dev/ttyUSB1 | grep -E '^ID_PATH=|^SUBSYSTEM='
ID_PATH=platform-20980000.usb-usb-0:1.3:1.0
SUBSYSTEM=tty
```

SUBSYSTEM should be always tty and ID_PATH identifies uniquely each adapter and must change.

Now create a new file under /etc/udev/rules.d/
I called my file amigaserials.rules.
Insert a line like this for each devfile

```
SUBSYSTEM=="#SUBSYSTEM#", ENV{ID_PATH}=="#PATH_ID#, SYMLINK+="#FILENAME#"
```

Of course you will have to replace #SUBSYSTEM# and #PATH_ID# with the values you got from the udevadm command and #FILENAME# must contain your logic name you want to create.

Here is my devfile for reference

    pi@raspberrypi /etc/udev/rules.d $ cat amigaserials.rules 
    SUBSYSTEM=="tty", ENV{ID_PATH}=="platform-20980000.usb-usb-0:1.2:1.0", SYMLINK+="ttyA500"
    SUBSYSTEM=="tty", ENV{ID_PATH}=="platform-20980000.usb-usb-0:1.3:1.0", SYMLINK+="ttyA600"
    SUBSYSTEM=="tty", ENV{ID_PATH}=="platform-20980000.usb-usb-0:1.5:1.0", SYMLINK+="ttyVampire"
    SUBSYSTEM=="tty", ENV{ID_PATH}=="platform-20980000.usb-usb-0:1.4:1.0", SYMLINK+="ttyA1200"
    
At next reboot you should see your new logic names under /dev.

    pi@raspberrypi ~ $ ls -latr /dev/tty* | grep "\->"
    lrwxrwxrwx 1 root root          7 Jan  1  1970 /dev/ttyA600 -> ttyUSB1
    lrwxrwxrwx 1 root root          7 Jan  1  1970 /dev/ttyA500 -> ttyUSB0
    lrwxrwxrwx 1 root root          7 Jan  1  1970 /dev/ttyA1200 -> ttyUSB2
    lrwxrwxrwx 1 root root          7 Jan  1  1970 /dev/ttyVampire -> ttyUSB3
    


