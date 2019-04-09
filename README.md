# Disclaimer

the dd command can wipe out all the content of a disk. I decline any responsability for the loss of any data. Please make due diligence

> CAREFULLY check if your usb is located on the sda, sdb or other port and replace the following below with the right one !!

# Create a Kali Linux Live boot key

First download the [kali-linux-2019.1a-amd64.iso image file](https://docs.kali.org/introduction/download-official-kali-linux-images)

Check Kali .iso file integrity:
```bash
echo "2d23cf0b35285ba68111154f169efa87fbb9ff618e68ea4cf6bd1023215d063e  kali-linux-2019.1a-amd64.iso" | sha256sum --check
```

Check also file with
```bash
gpg --keyserver hkp://keys.gnupg.net --recv-key 7D8D0BF6
gpg --fingerprint 7D8D0BF6
```

**If the file integrity is okay you can then proceeed to the installation**

locate port by running this command twice (before and after inserting usb key) and checking which device will appear (sda, sdb or whatever)
```bash
sudo fdisk -l
```

Install on the USB key (be very careful to indicate sda or sdb according to the difference you noticed in the previous step)
```bash
dd if=kali-linux-2019.1a-amd64.iso of=/dev/sda bs=512k
```
This last step should take few minutes

## Create persistence on Kali Linux Live boot key

https://docs.kali.org/downloading/kali-linux-live-usb-persistence

```bash
sudo su
```

Create the storage disk:
```bash
end=7gb
read start _ < <(du -bcm kali-linux-2019.1a-amd64.iso | tail -1); echo $start
parted /dev/sda mkpart primary $start $end
```

Create the persistence file
```bash
mkfs.ext3 -L persistence /dev/sda3
e2label /dev/sda3 persistence
```

Add it to the file system
```bash
mkdir -p /mnt/my_usb
mount /dev/sda3 /mnt/my_usb
echo "/ union" > /mnt/my_usb/persistence.conf
umount /dev/sda3
```
## In case you want to check where your data are in order to make a copy beforehand

locate usb key folder:
```bash
lsblk
```

## Any issue encountered ?

Please refer to [official site](https://www.kali.org/downloads/) / google / stackoverflow
