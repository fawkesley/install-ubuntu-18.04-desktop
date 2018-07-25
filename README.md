# Install / restore Ubuntu 18.04 with backintime snapshot

## Prerequisites

- A full backup of `/home/paul` using `backintime`


## Install Ubuntu 18.04

- Choose a minimal install (few system packages)

- Choose "Something else" to enter the partitioning tool

- Select "use the advanced partitioning tool for more control"
  - note: use `primary` partitions, not `logical`
  - add `/dev/sda1` as `ext4` size `2046` mountpoint `/boot`
  - in `sda` free space, select `physical volume for encryption` - it will create a new device `/dev/mapper/sda1_crypt
  - set `/dev/mapper/sda1_crypt` mountpoint to `/`
  - in `sdb` free space select `physical volume for encryption` - it will create a new device `/dev/mapper/sdb1_crypt`
  - set `/dev/mapper/sda5_crypt` mountpoint to `/home`

In the end it should look like this:

```
                                          fmt
/dev/mapper/sda2_crypt
  /dev/mapper/sda2_crypt1   ext4  /       [x]

/dev/mapper/sdb1_crypt
  /dev/mapper/sdb1_crypt1   ext4  /home   [x]

/dev/sda
  /dev/sda1                 ext4  /boot   [x]
  /dev/sda2                 unknown (cryptodisk)

/dev/sdb
  /dev/sdb1                 unknown (cryptodisk)
```

- on the timezone screen, *wait* for 30 seconds to allow the partitioner to do its thing (or error)
- Once the installer has finished, reboot

## Install system packages

```
wget https://github.com/paulfurley/install-ubuntu-18.04-desktop/archive/master.zip && unzip master.zip
sudo ./install-ubuntu-18.04-desktop/install
```

## Restore `/home/paul` from backintime

 - `sudo apt install backintime-common`
 - `backintime-qt4`
 - select Yes to "restore a previous configuration?"
 - navigate to the backup restore location
 - select the right backintime profile
 - from `Restore` select `Restore /home/paul`
 - ... wait a while
 - make sure it finished OK

## Finish setting up user

 - Reinstate crontab: `crontab < ~/crontab.txt`

## Reinstate`~/dotfiles`

- `cd ~/dotfiles && make`
- reload backintime config from `~/binfiles`
