---
layout: post
title: How-to mount E01 images in the SIFT workstation
---

For this how-to, I will be using the forensic image provided by Cal Poly from their [Windows and Android Forensics CCIC Training](https://cci.calpoly.edu/2019-digital-forensics-downloads), but you can use any E01 image you find and follow along. 

First, let us go over what an E01 image is. ASR Data developed the Expert witness compression format (E01) to store forensic images. 

For a forensic investigator to examine any digital evidence in E01, they need to mount the file. Using the [SIFT Workstation VM](https://www.sans.org/tools/sift-workstation/) we have access to ewfmount, mmls, and mount commands. These commands will help us access basic information about the image.

**NOTE**
If this is your first time using Linux, refer to the [manual page](https://www.man7.org/linux/man-pages/index.html) of each command before using them.

# Steps

- In your SIFT Workstation, navigate to the directory that contains the E01 image you will be using. 

![image1](/images/20210923200120SIFTVM.png)

- Use the ewfmount command to mount the E01 image to /mnt/ewf. 

```
ewfmount thenameofyourimage.E01 /mnt/ewf
```

![image2](/images/20210923200551SIFTVM.png)

- Make sure the image was mounted

```
ls -lah /mnt/ewf
```

![image3](/images/20210924112739SIFTVM.png)

- View the structure of the image that you mounted 

```
mmls /mnt/ewf/ewf1
```

![image4](/images/20210924114523SIFTVM.png)

**NOTE**
Pay attention to the size of sectors. In my example, they were 512-byte sectors.

The first partition begins at sector 2048 and ends at 0125827071

- Mount the partition starting at 2048 using the mount command with the following flags

```
mount -t ntfs-3g -o loop,ro,show_sys_files,stream_interface=windows,offset=$((2048*512)) /mnt/ewf/ewf1 /mnt/windows_mount/
```

![image5](/images/20210924160210SIFTVM.png)

- You will now be able to list out the directories of the Windows OS partition.

```
ls -lah /mnt/windows_mount/
```

![image6](/images/20210924155439SIFTVM.png)

The partitions will now be accessible from Files. 

![image7](/images/20210924161922SIFTVM.png)

To unmount the images, use the umount command and work in reverse. 

![image8](/images/20210924164438SIFTVM.png)

Stay tuned for future posts going over the artifacts from this image that you can examine using SIFT.

# References  

http://www.asrdata.com/whitepaper-html/

https://countuponsecurity.com/2015/11/09/digital-forensics-evidence-acquisition-and-ewf-mounting/

