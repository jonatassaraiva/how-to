# NVMe SSD Volumes

## lspci command
```
> lspci
```

## Verify that your operating system and device support TRIM
```
> sudo cat /sys/block/xvdb/queue/discard_max_bytes
```
If this command returns a value other than 0, then your operating system and device support TRIM.

## Format the volume
```
> sudo mkfs.xfs -K /dev/xvdb
```

## Mount the device
```
> sudo mkdir /mnt/data
> sudo mount -o discard /dev/xvdb /mnt/data
```