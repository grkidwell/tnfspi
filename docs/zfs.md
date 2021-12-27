# ZFS
## &nbsp;
Installation of the ZFS filesystem is optional and requires the Ubuntu 64bit operating system.

## Install zfsutils-linux
    pi@tnfspi:~$ sudo apt install zfsutils-linux

## Create zpool on zdata partition
    pi@tnfspi:~$ sudo zpool create bytor -f -d /dev/disk/by-label/zdata
    pi@tnfspi:~$ sudo zpool upgrade bytor

## Create tnfs zfs dataset
    pi@tnfspi:~$ sudo zfs create -o atime=off -o compression=lz4 -o recordsize=1k bytor/tnfs
limit zfs memory cache to 1GB
#
    pi@tnfspi:~$ sudo echo "options zfs zfs_arc_max=1073741824"|sudo tee -a /etc/modprobe.d/zfs.conf

## Install sanoid
    pi@tnfspi:~$ sudo apt install sanoid
    pi@tnfspi:~$ sudo mkdir /etc/sanoid
    pi@tnfspi:~$ sudo cp /usr/share/doc/sanoid/examples/sanoid.conf /etc/sanoid/sanoid.conf

## Edit sanoid config file
    pi@tnfspi:~$ sudo nano /etc/sanoid/sanoid.conf

Comment out all examples, add single entry for bytor/tnfs

Modify template_production frequently=10 and yearly=3

```
######################################
# This is a sample sanoid.conf file. #
# It should go in /etc/sanoid.       #
######################################

# name your backup modules with the path to their ZFS dataset - no leading slash.
#[zpoolname/datasetname]
        # pick one or more templates - they're defined (and editable) below. Comma separated, processed in order.
        # in this example, template_demo's daily value overrides template_production's daily value.
#       use_template = production,demo

        # if you want to, you can override settings in the template directly inside module definitions like this.
        # in this example, we override the template to only keep 12 hourly and 1 monthly snapshot for this dataset.
#       hourly = 12
#       monthly = 1

# you can also handle datasets recursively.
#[zpoolname/parent]
#       use_template = production
#       recursive = yes
        # if you want sanoid to manage the child datasets but leave this one alone, set process_children_only.
#       process_children_only = yes

# you can selectively override settings for child datasets which already fall under a recursive definition.
#[zpoolname/parent/child]
        # child datasets already initialized won't be wiped out, so if you use a new template, it will
        # only override the values already set by the parent template, not replace it completely.
#       use_template = demo

[bytor/tnfs]
        use_template = production
        recursive = yes



#############################
# templates below this line #
#############################

# name your templates template_templatename. you can create your own, and use them in your module definitions above.

[template_demo]
        daily = 60

[template_production]
        frequently = 10
        hourly = 36
        daily = 30
        monthly = 3
        yearly = 3
        autosnap = yes
```

## List snapshots
This is after about an hour or so
```
pi@tnfspi:~$ zfs list -t snapshot bytor/tnfs
NAME                                                 USED  AVAIL     REFER  MOUNTPOINT
bytor/tnfs@autosnap_2021-12-27_13:00:04_yearly         0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_13:00:04_monthly        0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_13:00:04_daily          0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_13:00:04_hourly         0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_13:00:04_frequently     0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_13:15:13_frequently     0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_13:30:19_frequently     0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_13:45:06_frequently     0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_14:00:22_hourly         0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_14:00:22_frequently     0B      -       24K  -
bytor/tnfs@autosnap_2021-12-27_14:15:00_frequently     0B      -       24K  -

```
## Access snapshots
```
pi@tnfspi:~$ ls /bytor/tnfs/.zfs/snapshot
autosnap_2021-12-27_13:00:04_daily       autosnap_2021-12-27_13:00:04_monthly     autosnap_2021-12-27_13:30:19_frequently  autosnap_2021-12-27_14:00:22_hourly
autosnap_2021-12-27_13:00:04_frequently  autosnap_2021-12-27_13:00:04_yearly      autosnap_2021-12-27_13:45:06_frequently  autosnap_2021-12-27_14:15:00_frequently
autosnap_2021-12-27_13:00:04_hourly      autosnap_2021-12-27_13:15:13_frequently  autosnap_2021-12-27_14:00:22_frequently

```
This page under construction