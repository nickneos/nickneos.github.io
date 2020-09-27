---
# layout: post
title: Schedule Home Assistant Config Backups
tagline: Using cron to schedule Home Assistant config backups on a linux hosted system
date: 2020-09-18
header:
  overlay_image: /assets/images/blog/cron.jpg
  overlay_filter: 0.5
  teaser: /assets/images/blog/cron.jpg
categories:
  - Home-Assistant
tags:
  - home-assistant
  - docker
  - linux
  - backup
  - cron
published: true
canonical_url: https://www.nickneos.com/2020/09/18/schedule-home-assistant-config-backups/
---


In a [previous post](https://www.nickneos.com/2020/09/14/migrating-home-assistant/) I went through the process of installing Home Assistant Core onto your own Docker environment on a Linux system.

When I used to have Home Assistant installed on a Raspberry Pi I was using [Hass.io Google Drive Backups Addon](https://github.com/sabeechen/hassio-google-drive-backup) to regularly backup my HA config. Now that I have HA Core on my own docker environment, this addon is not available to use. But it should be easy enough to create our own backup system in linux and use cron to schedule. Here I will walk through how I did this.

## Create a Backup Script

First let's create a folder and go into that folder:
```bash
mkdir ~/HA-Backup/
cd ~/HA-Backup
```
Create a shell script by executing: `nano ha-backup.sh` and paste in the below:

```bash
#!/usr/bin/env bash

HA_CONFIG="~/docker/homeassistant/config/"  # location of your HA config
DOM=$(date +%d)
DOW=$(date +%u)
TIMESTAMP=$(date +%Y%m%d)

### Backup Rotation settings. Change as needed
ROT_DAYS=3     # keep the last 3 daily backups
ROT_WEEKS=4    # keep the last 4 weekly backups
ROT_MONTHS=12  # keep the last 12 yearly backups

 mkdir -p daily/
 mkdir -p weekly/
 mkdir -p monthly/

# determine folder for backup - daily, weekly or monthly
if [ "$DOM" -eq 1 ]; then
    DEST="./monthly/"
elif [ "$DOW" -eq 1 ]; then
    DEST="./weekly/"
else
    DEST="./daily/"
fi

# Create Backup
tar -czvf haconfig_$TIMESTAMP.tar.gz -C $HA_CONFIG .
mv -fv haconfig_$TIMESTAMP.tar.gz $DEST/

# Clean up daily files older than x days 
DAYS=$((ROT_DAYS * 1))
find ./daily/ -type f -mtime +$DAYS -exec rm -fv {} \;

# Clean up weekly files older than x weeks 
DAYS=$((ROT_WEEKS * 7))
find ./weekly/ -type f -mtime +$DAYS -exec rm -fv {} \;

# Clean up monthly files older than x months 
DAYS=$((ROT_MONTHS * 31))
find ./monthly/ -type f -mtime +$DAYS -exec rm -fv {} \;

```

Make sure to change `HA_CONFIG` line to your HA config directory.

`ROT_DAYS`, `ROT_WEEKS` and `ROT_MONTHS` are the rotation backup settings and can be changed based on how many backups you want to keep.

When done, save the file and exit Nano `ctrl x`

Give the shell script execute permissions:
```bash
chmod +x ha-backup.sh
```

## What the backup script does

This script will create a tar.gz backup of the Home Assistant config directory. It will move the tar.gz backup as follows:

* If it's the first day of the month, it will move the backup to `./monthly/` folder
* Otherwise if it's a Monday, it will move the backup to `./weekly/` folder
* Otherwise it will move the backup to `./daily/` folder

It will then delete any files older than x days in the daily, weekly, and monthly folders based on `ROT_DAYS`, `ROT_WEEKS` and `ROT_MONTHS` variables in the shell script.

Below screenshot shows all the backups on my system across since around 2 months ago. 

![Backups](/assets/images/blog/ha-backup-files.jpg)

## Running the script

You should now be able to test out executing the script.

```bash
./ha-backup.sh
```

You should now see a tar.gz backup in either the daily, weekly or monthly folder.

## Scheduling the script in Cron

In a terminal run the following:

```bash
sudo crontab -e
```

Now add the following to the end of the file, making sure to modify the `/path/to/HA-Backup` part (NOTE: don't use `~` in your path, since this will be run as root user which will have a different home directory):

```bash
30 4 * * * cd /path/to/HA-Backup && ./ha-backup.sh > /dev/null 2>&1
```

This will run the script daily at 4:30am. Change this as needed (eg. to run daily at 11:59pm change `30 4 * * *` to `59 23 * * *`)

Hit `ctrl x` to save and exit nano.

Cron should now be updated with this schedule.

# Restoring a backup

If you ever need to restore a backup, do the following.

Stop Home Assistant:
```bash
docker stop homeassistant
```

Rename your existing Home Assistant config folder (in case you need to go back to it), and create a new empty config folder (adjust path based on your config location):
```bash
sudo mv /docker/hommeassistant/config/ /docker/hommeassistant/config_old/ 
mkdir /docker/hommeassistant/config/
```

Unzip the backup. In this example we are unzipping `~/HA-Backup/daily/haconfig_20200901.tar.gz` to our Home Assistant config folder `/docker/hommeassistant/config/` (adjust path based on your config location).

```bash
sudo tar -xzvf ~/HA-Backup/daily/haconfig_20200901.tar.gz -C /docker/hommeassistant/config/
```

Note that running the above command with sudo, ensures files extracted preserve the permissions they originally had.

You can now start up Home Assistant again, and it will be using the restored config:
```bash
docker start homeassistant
```

