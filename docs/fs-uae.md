In this section I'm going to describe how to use alsfs in an emulated environment using fs-uae and a virtal serial port using socat,this will allow you to perform a painless test through different emulated amigas very quickly.

Requirements:

- A modern desktop/laptop machine FS-UAE installedWorkbench (at least 2.1) adf files
- A 64bit Gnu/Linux server machine (no matter if virtual or not) with Docker installed
- The 2 machines must be able to talk through an ip connection.

Step 1:
Create a new fs-uae configuration with fs-uae launcher, this configuration must be able to boot workbench 2.1 or above.

Step 2: Open your configuration file with your text editor, usually it is located under ~/Documenti/FS-UAE/Configurations/

and append the serial port parameter line in the form tcp://ip:port/wait at the end of the file, for example:

```
  # FS-UAE configuration saved by FS-UAE Launcher
  # Last saved: 2017-12-17 18:26:49

  [fs-uae]
  amiga_model = A1200
  chip_memory = 2048
  cpu = 68EC020
  fast_memory = 8192
  ...
  serial_port = tcp://192.168.137.2:1234/wait
```

This will instruct fs-uae to wait to boot the amiga until a tcp connection with 192.168.137.2 port 1234 is established.
Please note that if you enter and invalid serial port (for example you don't have any network interfaces with this ip assigned), fs-uae will boot Workbench without waiting, this must not occur.
In this scenario we are using ip 192.168.137.2 for the fs-uae side and 192.168.137.3 for the server side, as stated previously this 2 hosts must be able to talk to each other.

Start the fs-uae amiga machine, the boot process will be freezed, waiting for the tcp connection to come up.

Go to the Gnu/Linux machine (192.168.137.3) with Docker installed and run:

```
  docker run --rm -it -p 8081:8081 -p 1234:1234 -v /alsfsAmigaServer/:/server -w /server node /bin/bash
```

Of course you must install alsfsAmigaServer on the directory /alsfsAmigaServer before running this command.

Now let's install socat, a powerful software which lets you creare a virtual tcp socket necessary to establish a connection with fs-uae running on your laptop.

```
  apt-get update && apt-get install socat
  socat pty,link=/dev/virtualcom0,raw tcp:192.168.137.2:1234 &
```

This command will create a new file (/dev/virtualcom), every tcp packet for 192.168.137.2:1234 will be routed to this file that emulates a serial connection, this will be our connection between alsfs server and fs-uae.
After running socat, fs-uae must start the Workbench boot sequence, at this point you can bootstrap alsfs or, if you have alsfssrv already installed on your emulated amiga, start the alsfs nodejs server, in this scenario the command will be:

```
node amigajsserver.js /dev/virtualcom0 0.0.0.0
```

Since the default listening port is 8081, this command is equivalent to 

```
node amigajsserver.js /dev/virtualcom0 0.0.0.0:8081
```

The same 8081 port is exposed by docker using the -p parameter when we created the virtual container.

Once the alsfs nodejs server is started you can start mounting alsfs partitions on your desktop/laptop machine, or any third Gnu/Linux machine sitting on your network, all you have to do is to point alsfs with the server machine running Docker, in this scenario it would be:

```
/usr/local/bin/alsfs 192.168.137.3:8081 ~/amigapi
```

