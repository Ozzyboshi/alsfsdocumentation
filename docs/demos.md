# Demos
All vides in this page are shot using a virtual A1200 and a "vitual serial cable" to ease the video production but you will get the same results with real Commodore Amigas.

### Alsfs filesystem interaction
This is a little demo of my project.
An emulated Amiga is running Workbench 2.1 and a Linux PC is accessing the Amiga volumes using a virtual filesystem.
The Amiga is communicating with the PC machine through a virtual serial cable.
Of course the whole system works with real Amigas.

In the first part of the demo a text file is created using a Linux editor but saved directly into the Amiga ramdisk.
Then a mod file is copied into the Amiga ramdisk and loaded into protracker.
The demo also shows the create/delete feature of files and directory.

[![Alsfs filesystem interaction](https://img.youtube.com/vi/HHleQ2CO39Y/0.jpg)](https://www.youtube.com/watch?v=HHleQ2CO39Y)

### Alsfs adf writing through chrome extension
In this video you can see the alsfs chrome extension in action.
After right clicking on a web hosted zipfile containing an adf image, the extension takes care of decompressing and directly uploading to a real amiga using the serial cable.
In this demo the amiga and the serial cable are virtualized.
After about 10 minutes we have the full adf image written into df0.

[![Chrome adf writing](https://img.youtube.com/vi/Epe-nVQ9MEo/0.jpg)](https://www.youtube.com/watch?v=Epe-nVQ9MEo)

### Alsfs adf writing through filesystem
Another way to write an adf file to an Amiga floppy disk, the Amiga floppy drive is mounted under ~/amiga/adf/DF0, just copy the adf file in this folder to start the writing process.

[![Alsfs adf writing through filesystem](https://img.youtube.com/vi/IXfTbbD9-8w/0.jpg)](https://www.youtube.com/watch?v=IXfTbbD9-8w)


