<img align="right" src="https://github.com/user-attachments/assets/72574934-26dc-426e-bce6-1ac09027caf6" width="350" alt="Mainline kernel on Redmi Note 9 Pro with failing boot">

# Running Linux on the Redmi Note 9 Pro / 9 Pro India / 10 Lite / 9S / 9 Pro Max India / Poco M2 Pro

## ⚠️ Warning
If you are newer to Linux, and not comfortable while flashing your device something custom, you SHOULD be stop reading this guide.

This is for only experienced peoples, or testers.

You can find project status in [here.](status.md)

## Requirements
- A Ubuntu PC
- At least 12GB RAM (Also can go with 8GB but build will be slower)
- At least 50GB NVME M.2 SSD (SATA will work but will be a lot slower)
- AMD Ryzen 5 3th Gen or newer, or Intel i5 9400F or newer.
- Some time (example on Ryzen 5 3600 24GB RAM 500GB NVME M.2 SSD, build tooks 20 minutes, so yeah. Prepare yourself.)

## Getting the basic tools
First, you need these packages: ```git, curl, wget, zip, bc, rsync, build-essential libncurses-dev bison flex libssl-dev libelf-dev cpython3```

Then, download a MIUI firmware for your device, and extract boot.img from it. Create a temporary folder.

Download magiskboot from [here.](https://raw.githubusercontent.com/Uevo001/magiskboot-linux/refs/heads/main/x86_64/magiskboot) This is for x86_64 but you can find other arch if you google it.

Unpack your boot.img with this command:
```magiskboot unpack boot.img```

Now you have the basics. A unpacked boot.img, magiskboot, ```dtb, kernel and ramdisk``` file.

## Cloning the sm7125-mainline linux repository.
You can clone it by running this command on terminal: ```git clone https://github.com/sm7125-mainline/linux --depth=1```

## Building linux kernel
After the cloning is finished, cd to it. ```cd linux```

Then start configuring the kernel with this command: ```make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- defconfig sm7125.config```

And after that, start building the kernel: ```make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- -j$(nproc) Image.gz dtbs modules```

After finishing, you can finally boot the Linux kernel. Now go to temporary folder that you unpacked boot.img.

Delete dtb and kernel with this command: ```rm -f dtb && rm -f kernel```

Now, copy the dtb file that matches your device. You can list dtb's for sm7125 with this command: ```ls ~/linux/arch/arm64/boot/dts/qcom/sm7125-```

For example I'm going to use huaxing panel, joyeuse. Let's copy the correct dtb into our temporary folder: ```cp ~/linux/arch/arm64/boot/dts/qcom/sm7125-xiaomi-joyeuse-huaxing.dtb dtb```

Finally, copy the Image.gz as kernel: ```cp ~/linux/arch/arm64/boot/Image.gz kernel```

Now you are done with the linux part. Repack your boot image with this command (make sure you didn't remove the old boot.img) ```magiskboot repack boot.img```

You are ready to flash right now. Use these commands to flash: 

```fastboot erase dtbo```
```fastboot flash boot boot.img```

Now wait for the Tux logo on top of the screen. There you go! You learned a new thing: How to build and boot Linux kernel.
