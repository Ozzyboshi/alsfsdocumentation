# Alsfssrv
Alsfssrv is the Amiga counterpart of the alsfsNodejsServer, It sits in background waiting for someone's requests through the Amiga serial port, responses are also dispatched using the serial port.
Each request must have a very specific syntax, in many cases, where some sort of synchronization is required, the requests start a dialog demanding the calling part more information.

### How to compile
Although you can get alsfssrv in his Amiga binary form from the [alsfsNodejsServer Github page](https://github.com/Ozzyboshi/alsfsNodejsServer), It can be useful to compile it, especially if you want to apply your own patches or new features.

In order to compile alsfssrv I used [vbcc C Compiler](http://sun.hasenbraten.de/vbcc/), I found the instruction how to setup the environment on [this youtube video](https://www.youtube.com/watch?v=vFV0oEyY92I&t=50s).

However the entire process is time consuming and error prone, so I dockerized it creating my own vbcc Docker image.

You can compile the whole alsfssrv sources with the following commands:

```
wget https://github.com/Ozzyboshi/alsfsAmigaServer/archive/v1.tar.gz
tar -xvzpf v1.tar.gz
cd alsfsAmigaServer-1/
docker run -it -v `pwd`:/data --rm  ozzyboshi/dockeramigavbcc make -f /data/Makefile
ls -ls alsfssrv
28 -rwxr-xr-x 1 root root 28608 nov  7 10:08 alsfssrv
file alsfssrv
alsfssrv: AmigaOS loadseg()ble executable/binary
```

### List of command supported by Alsfssrv

* vols: returns the volumes mounted on the Amiga
* device: returns the devices found on the Amiga
* stat: returns informations about a file or a drawer on a mounted amiga filesystem like size and date time of creation
* list: returns a list of files and drawers found at a location on a mounted amiga filesystem
* storeraw: stores raw data to a file
* createfile: create an empty file
* mkdrawer: create an empty drawer (directory)
* rename: renames a file or a drawer
* readfile: read raw data from a file
* delay: puts the amiga in a wait state
* delete: deletes a file or a drawer
* writeadf: writes an adf image to a floppy disk
* readadf: reads an adf image from a floppy disk
* checkfloppy: checks if a floppy drive is in the list of devices found on the Amiga
* relabel: changes the label of a floppy disk or a mounted volume
* exit: stop the alsfssrv application
