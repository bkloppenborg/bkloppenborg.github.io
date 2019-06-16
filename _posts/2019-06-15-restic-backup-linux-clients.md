---
layout: post
title: "Setup, configuration, and automation of restic for Linux clients"
date: 2019-06-15T20:57:00-07:00
draft: false
author: "Brian Kloppenborg"
categories: ["backups"]
tags: ["backups", "restic"]
--- 

This is the third post in a four-part series which discusses my backup solution
which uses dedicated backup machines, restic, and the restic REST server.
In my first post, I discuss how I decided to use restic rather than other backup
clients after 
[evaluating various backup solutions]({% post_url 2019-06-08-six-years-evaluating-backup-software %})
over the last six years. In the 
[second post]({% post_url 2019-06-12-restic-backup-server %}),
I setup a dedicated backup server with a `btrfs` RAID1 filesystem, automate
`btrfs` scrubbing using systemd timers, install `restic`, and install the restic
REST server.

This post explains how to install restic on Linux systems, create configuration
scripts to simplify the backup process, and automate said backups using systemd
timers.

## Restic Installation

As explained in the previous post, Ubuntu 18.04 ships with restic 0.8.3 which
is not as easily automated as later releases. As such, I suggest you download
the latest version of restic from the
[restic GitHub repository](https://github.com/restic/restic/releases)
and copy the restic binary to `/usr/local/bin`:

    sudo apt install bunzip2
    wget https://github.com/restic/restic/releases/download/v0.9.5/restic_0.9.5_linux_amd64.bz2
    bunzip2 restic*.bz2
    sudo cp restic*amd64 /usr/local/bin/restic

## Create configuration file

To simplify the backup process, we are going to create a file to store
environmental variables that are used by restic during the backup process.
I store these files in the `~/local/restic` directory; however, you can
place them where ever convenient.

Create a `~/local/restic/restic-env.sh` with tNEXT                         LEFT     LAST                         PASSED     UNIT                ACTIVATES
Mon 2019-06-17 00:00:00 MDT  13h left Mon 2019-06-10 00:00:40 MDT  6 days ago restic-weekly.timer restic-weekly.service
he following content:

    REST_USER="rest_username"
    REST_PASS="some_awesome_password"
    REST_REPO="restic_repository"
    REST_SERV="backup_server_fqdn"
    
    # Restic repository credentials
    export RESTIC_REPOSITORY="rest:http://${REST_USER}:${REST_PASS}@${REST_SERV}:8000/${REST_REPO}"
    export RESTIC_PASSWORD="awesome_password2"

Update the `REST_USER`, `REST_PASS`, `REST_REPO`, `REST_SERV`, and
`RESTIC_PASSWORD` variables to match your configuration. The variables prefixed
in `REST` are for the restic REST server whereas the `RESTIC_PASSWORD` is for
the restic repository itself.

Next we need to initialize the repository on the remote machine. From the `bash`
shell:

    cd ~/local/restic/
    source restic-env.sh
    restic init
    
If this works, then your script configuration is correct. Next, attempt to run
a backup of some (small) directory:

    source restic-env.sh
    restic backup some_source_directory

Then check the integrity of a backup:

    source restic-env.sh
    restic check
    
## Script the backup process

Now we will automate the backup process by creating a script to execute and a
systemd user timer to run said script.

Create the `~/local/restic/restic-backup.sh` file with the following contents:

    #!/bin/bash
    BASE_DIR="$(cd "$(dirname "$0")" && pwd)"
    SCRIPT_DIR="$(dirname "$(readlink -f "$0")")"

    2>&1

    echo "Running Restic backup script for $USER on `date`"

    # Import the configuration settings.
    source $SCRIPT_DIR/restic-env.sh

    # Run the backup
    restic backup                \
        /home/$USER/Desktop      \
        /home/$USER/Documents    \
        /home/$USER/local        \
        /home/$USER/Music        \
        /home/$USER/Pictures     \
        /home/$USER/Private      \
        /home/$USER/Projects     \
        /home/$USER/workspace    \
        /home/$USER/.ssh

    # Remove old backups. Enable if your REST server is not in append only mode.
    #restic forget           \
    #       --keep-hourly 8  \
    #       --keep-daily 7   \
    #       --keep-weekly 4  \
    #       --keep-monthly 6 \
    #       --keep-yearly 10

    # check that the backup is ok
    restic check

    # reset credentials
    unset RESTIC_REPOSITORY
    unset RESTIC_PASSWORD

    exit 0

Be sure to modify the script to ensure the directories you wish to back up are
properly listed. `chmod +x` the backup file and execute it once to ensure it 
runs. Verify that the backup succeeded by inspecting the output of
`restic snapshots`:

    source restic-env.sh
    restic snapshots

## Automation using systemd user-mode timers/services (users)

To automate the backup process, we are going to use systemd services and timers.
The instructions in this section are written for user accounts. If you are backing
up a system directory, skip to
[the next section](#automation-using-systemd-user-mode-timersservices-system)

First, create a `restic-weekly.service` file with the following contents:

    [Unit]
    Description=Restic User Backup
     
    [Service]
    Type=simple
    Nice=19
    IOSchedulingClass=2
    IOSchedulingPriority=7
    ExecStart=/home/%u/local/restic/restic-backup.sh

Next create a `restic-weekly.timer` file with the following contents:

    [Unit]
    Description=Restic User Backup Timer
     
    [Timer]
    WakeSystem=false
    OnCalendar=weekly
    Persistent=true
     
    [Install]
    WantedBy=timers.target
    
Check the files for correctness using:

    systemd-analyze --user verify restic-weekly.service
    systemd-analyze --user verify restic-weekly.timer
    
This should return without error (note that Ubuntu 18.04 ships with `systemd` 237
which will always return a "Attempted to remove disk file system, and we can't allow that.")

Copy files to the `~/.config/systemd/user/` directory, enable, and start
the timer

    mkdir -p ~/.config/systemd/user/
    cp restic-weekly.* ~/.config/systemd/user/
    systemctl --user enable restic-weekly.timer
    systemctl --user start restic-weekly.timer
    
Ensure that the timer will be scheduled using `systemctl`:

    systemctl --user list-timers --all
    NEXT                         LEFT     LAST                         PASSED     UNIT                ACTIVATES
    Mon 2019-06-17 00:00:00 MDT  13h left Mon 2019-06-10 00:00:40 MDT  6 days ago restic-weekly.timer restic-weekly.service

To check the status of the backup in the future, use `systemctl` as well:

    systemctl --user status restic-weekly.service

## Automation using systemd user-mode timers/services (system)

If you are going to automate several directories, I would suggest setting
up a systemd template unit file instead. In this case, follow the instructions
above except use the following `service` and `timer` files:

First, the service: save as `restic-weekly@.service`

    [Unit]
    Description=Restic Backup
     
    [Service]
    Type=simple
    Nice=19
    IOSchedulingClass=2
    IOSchedulingPriority=7
    ExecStart=%f/restic/restic-backup.sh
    
Note that this script will run a backup on the location specified in the
systemd configuration line below, but it expects there to be a `restic` directory
containing the environment configuration file and `restic-backup.sh` file.

Next the timer: save as `restic-weekly@.timer`

    [Unit]
    Description=Restic Backup Timer
     
    [Timer]
    WakeSystem=false
    OnCalendar=weekly
    Persistent=true
     
    [Install]
    WantedBy=timers.target
    
Check the files for correctness using:

    systemd-analyze verify restic-weekly@mnt-storage.service
    systemd-analyze verify restic-weekly@mnt-storage.timer
    
Notice the `mnt-storage` entry on the last line. Systemd will interpret this
as an argument to the `restic-weekly@.service` file. In this case, the 
`mnt-storage` argument will be converted to `/mnt/storage` automatically
and provided to the `ExecStart` line.

If the above step went ok, copy the timer to the `/etc/systemd/system/`
directory, enable, and start the timer:

    sudo cp restic-weekly* /etc/systemd/system/
    sudo chmod 644 /etc/systemd/system/restic-weekly*
    sudo systemctl enable restic-weekly@mnt-storage.timer
    sudo systemctl start restic-weekly@mnt-storage.timer

Now, verify that the timer is operational/scheduled:

    sudo systemctl list-timers --all
    NEXT                         LEFT                LAST                         PASSED             UNIT                            ACTIVATES
    Sun 2019-06-16 17:39:02 UTC  58s left            n/a                          n/a                systemd-tmpfiles-clean.timer    systemd-tmpfiles-clean.service
    Sun 2019-06-16 18:03:06 UTC  25min left          Sun 2019-06-16 17:01:18 UTC  36min ago          anacron.timer                   anacron.service
    Mon 2019-06-17 00:00:00 UTC  6h left             Mon 2019-06-10 00:00:19 UTC  6 days ago         fstrim.timer                    fstrim.service
    Mon 2019-06-17 00:00:00 UTC  6h left             Sun 2019-06-16 17:18:57 UTC  19min ago          restic-weekly@mnt-storage.timer restic-weekly@mnt-storage.service
    ...
   
In the future you can check the status of the service using either of these commands:

    sudo systemctl status restic-weekly@mnt-storage.timer
    journalctl --unit restic-weekly@mnt-storage
