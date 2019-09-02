# Self Upgrades With Compressed Files



This demo shows how to use the built-in [fd.copyfirmwarelzo](http://en.wikipedia.org/wiki/Fw190) method for unzipping a compressed ROM image and flashing your device.

\##Why This Is a Big Deal

Most of our devices currently have 1MB of RAM. Once a project grows larger than 500KB, you will not be able to perform a remote upgrade of the project. There will simply not be enough space on the device to host the new version — if your current version takes 600KB, and the new version takes 610KB, you're stuck.

That is, you *were* stuck, until LZO compression came along. The download above contains three folders that show how this works. Work through them in the following sequence:

- **Blinker:** This is just a simple demo project. It blinks the LEDs. This is our payload for this demo — in real life, this would be your own application.

- LZO:

   

  This is where compression happens. This folder contains several files:

  - **blinkr.tpc:** The compiled version of the blinker project.
  - **tios-NB1010W-3_30_01:** A build of TiOS for a specific device. You will have to swap this for the [TiOS build](http://tibbo.com/downloads.html#tios) that is right for your own target device.
  - **FILLER.DAT:** This is just a 128-byte file needed for padding out the firmware when merging it with the TPC. Leave it as-is.
  - **lzo-1x-999.exe:** This is the open-source (GPL) [LZO](http://www.oberhumer.com/opensource/lzo/) compression utility.
  - **LZO.BAT:** This is a batch file that first combines the TPC and TiOS into one bin file, and then runs that file through LZO to compress it.
  - **blinker.bin:** If you've been tracking so far, you know this is the combined binary (TPC + TiOS).
  - **blinker_compressed.bin** You can probably guess what this is.

- **lzo_test:** This is a demo project showing how to work with fd.copyfirmwarelzo. For the purposes of this demo, we included the compressed BIN file as a resource file. Of course, in real life you're going to send it to your device over Ethernet, but we figured you already know how to do this, and it's not really the point here. Read this to see how to uncompress the file.

\##Why We Like LZO

In case you're curious about our choice of algorithm, here are some of the considerations that made us go for LZO:

- It's **fast**.
- It doesn't require complex memory management. Incoming buffer, outgoing buffer — that's it.
- It's basically ZIP. Actually, ZIP is LZO with some frills on top. Vanilla LZO compresses nearly as well as ZIP.
- It's open-source and free.