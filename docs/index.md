# Welcome to the Alsfs documentation website

The Amiga Linux Server FileSystem (alsfs from now) is a collection of applications for managing file transfers between a GNU/Linux PC and a real Amiga with a regular serial cable.

The final goal of alsfs is to mount a remote filesystem on your PC connected to your Amiga and see the while Amiga system like a regular partition with write and read permission on the ram disk, the floppy disk drives and the potential installed hard disks.

With alsfs you can:

* Upload files from your PC to your Amiga
* Download files from your PC to your Amiga
* Create and delete drawers (directories) on your Amiga
* Create, edit and delete files on your Amiga
* Get adf images from your real Amiga floppy
* Write adf images stored on your PC to a real Amiga floppy.

### Why?
This is a very good question, there are already a lot of solutions out there, why should I use alsfs?

The most common and popular piece of software for communicating with a real Amiga is of course [AmigaExplorer by Cloanto](https://www.amigaforever.com/ae/).

Amigaexplorer works very well and is awesome but it requires a Windows machine and and its code is not free.
Since I don't own a Windows machine (and I don't want to buy or run Microsoft operating systems in my hardware) I wrote some Linux compatible code to match Amigaexplorer most famous features, that's how alsfs is born.

Other solutions require you to take advantage of the pcmcia slot (available only on certain Amiga models) and buy additional hardware like ethernet cards or a compact flash adapter.
The advantage of alsfs is that it (hopefully) work with with the serial port of your Amiga (installed in every Amiga) and you don't have to buy new hardware, the only thing you need is a very cheap and easy to build null modem cable. 

On the other side I must warn you, alsfs is extremely slow if compared with Amigaexplorer but at the end of the day It works for me and I doubt I will come up with speed improvement in the future.

### Prerequisites

* A GNU/Linux based server with a serial port - This is meant to host a nodejs web server, any distro should be OK as long it's capable to run nodejs with expressjs.For this task I use a Raspberry Pie 1 and an Intel NUC, they are low powered and relatively inexpensive.
If you don't have a serial port available you can buy a [Manhattan da USB to Serial converter](http://www.manhattan-products.com/usb-to-serial-converter), this converter works well in my NUC and my Raspberry PI, I suppose it works on any GNU/Linux distribution with a relatively recent kernel.
* A null-modem serial cable - You can buy it online or build it on your own, [the schematics are here](https://www.amigaforever.com/gfx/null-modem/wiring_9-25.gif).
* A real commodore Amiga - I tested the whole project on my 2 A600, one is a stock version with just a MB of chip ram and no fast ram, the other is a little pumped with 2 MB of Chip ram and other 2 of fast through PCMCIA card and a 4 GB CF as mass storage.Alsfs works good om both of them.
* Workbench 2.1 floppy disks (the main one and the extras disk are enough). Workbench 1.3 won't work because Arexx is not included.
* A regular GNU/Linux PC able to run [fuse and libfuse](https://github.com/libfuse/libfuse) - You will use this PC to control the amiga, it can be the same you used for hosting the nodejs server if you prefer.

If you lack some of the prerequisites Is it possible to test alsfs in a fully emulated fashion using [fs-uae](https://fs-uae.net/) emulator in combination with [socat](https://linux.die.net/man/1/socat), the latter one is necessary to "emulate" the serial cable, more details how to do that in the "how to install" section.

One more thing: in order to run alsfs nodejs server I recommend using [docker](https://www.docker.com/) if possible.
Docker makes the installation painless and very quick

### What next?

If you are interested in alsfs you can give a look to some [demos](demos/) or at the "how install section".
Dont forget alsfs is an home made project and I am the only contributor.
Of course the code is not "the state of the art" but I accept suggestions and contributions of any kind.
Feel free to open issues on my [github page](https://github.com/Ozzyboshi).
