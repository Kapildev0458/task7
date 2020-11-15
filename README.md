# task7.1 part_1
# #vimaldaga #righteducation #educationredefine #rightmentor #worldrecordholder #linuxworld #makingindiafutureready #righeudcation #arthbylw
To create an LVM logical volume, the physical volumes are combined into a volume group (VG). This creates a pool of disk space out of which LVM logical volumes (LVs) can be allocated. This process is analogous to the way in which disks are divided into partitions. A logical volume is used by file systems and applications (such as databases).

 7.1: Elasticity Task

Integrating LVM with Hadoop and providing Elasticity to DataNode Storage

Increase or Decrease the Size of Static Partition in Linux.

Let's Attach a volume to your instance. 

![7 1](https://user-images.githubusercontent.com/69908356/99189348-c8a09480-2786-11eb-8d2b-33fe0a9c6232.png)

Now, let's check if the volume is attached or not :

![7 2](https://user-images.githubusercontent.com/69908356/99189347-c6d6d100-2786-11eb-828f-aadd178a4a73.png)

So we attached two volumes of 4 Gib and 2 Gib
Now let's create the Physical volumes(PV) of both volumes we attached.
First one :
pvcreate /dev/sdd

pvdisplay /dev/sdd

![7 14](https://user-images.githubusercontent.com/69908356/99189655-42854d80-2788-11eb-835f-ea6f87b8b1f2.png)

Second one :
pvcreate /dev/sdd

pvdisplay /dev/sdd

![7 3](https://user-images.githubusercontent.com/69908356/99189441-2c2ac200-2787-11eb-895e-e7d378154376.png)

We have To create a Volume Group(VG) of both the physical volumes
vgcreate task7.1 /dev/sdc /dev/sdd

vgdisplay task7.1


![7 4](https://user-images.githubusercontent.com/69908356/99189440-2b922b80-2787-11eb-87b0-f4986a55e9f9.png)

Let's create a logical volume(LV) of 3Gib from the above volume group.
lvcreate --size 3G --name mylv1 task7.1

lvdisplay task7.1/mylv1

![7 5](https://user-images.githubusercontent.com/69908356/99189439-2b922b80-2787-11eb-871e-c2d80ed6697c.png)

After creating the Logical volume we can use it as storage for the Datanode.
To use it as a Storage for DataNode follow the following steps.
Step 1:Create a folder in the root folder.

mkdir task7.1

![7 6](https://user-images.githubusercontent.com/69908356/99189437-2af99500-2787-11eb-972d-176bd34fd4a6.png)

Step 2: Mount the above folder to the logical volume.

mount /dev/task7.1/mylv1 task7.1

![7 7](https://user-images.githubusercontent.com/69908356/99189436-2a60fe80-2787-11eb-8b10-71b72bebc722.png)

Successfully Mounted
After mounting the folder configure it in hdfs-site.xml file.

![7 8](https://user-images.githubusercontent.com/69908356/99189435-2a60fe80-2787-11eb-9827-1215d486683a.png)

As we configured with 3GiB of the logical volume.

![7 9](https://user-images.githubusercontent.com/69908356/99189434-29c86800-2787-11eb-8efa-6989fc2b6b8b.png)



Let's increase the size of the logical volume :
Step 1: Extend it
lvextend --size +2G /dev/tasl7.1/mylv1
No alt text provided for
![7 10](https://user-images.githubusercontent.com/69908356/99189432-292fd180-2787-11eb-80e0-a596d3b540c4.png)
 this image
Step 2: Resize it
resize2fs /dev/task7.1/mylv1

![7 11](https://user-images.githubusercontent.com/69908356/99189430-28973b00-2787-11eb-8a90-393e049fba86.png)

The storage is Extended successfully on the fly.

![7 12](https://user-images.githubusercontent.com/69908356/99189429-27fea480-2787-11eb-9075-7ed3800d0b71.png)

Let's decrease the size of the logical volume :
lvreduce -r -L 1G /dev/task7.1/Mylv1

-r option defines to resize it

![7 13](https://user-images.githubusercontent.com/69908356/99189427-26cd7780-2787-11eb-8540-a9264933888c.png)
