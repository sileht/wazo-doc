# Backup

## Periodic backup

A backup of the database and the data are launched every day with a
logrotate task. It is run at 06:25 a.m. and backups are kept for 7 days.

Logrotate task:

> <span data-role="file">/etc/logrotate.d/xivo-backup</span>

Logrotate cron:

> <span data-role="file">/etc/cron.daily/logrotate</span>

## Retrieve the backup

With shell access, you can retrieve them in
<span data-role="file">/var/backups/xivo</span>. In this directory you
will find <span data-role="file">db.tgz</span> and
<span data-role="file">data.tgz</span> files for the database and data
backups.

Backup scripts:

> <span data-role="file">/usr/sbin/xivo-backup</span>

Backup location:

> <span data-role="file">/var/backups/xivo</span>

## What is actually backed-up?

### Data

Here is the list of folders and files that are backed-up:

  - <span data-role="file">/etc/asterisk/</span>
  - <span data-role="file">/etc/consul/</span>
  - <span data-role="file">/etc/crontab</span>
  - <span data-role="file">/etc/dahdi/</span>
  - <span data-role="file">/etc/dhcp/</span>
  - <span data-role="file">/etc/hostname</span>
  - <span data-role="file">/etc/hosts</span>
  - <span data-role="file">/etc/ldap/</span>
  - <span data-role="file">/etc/network/if-up.d/xivo-routes</span>
  - <span data-role="file">/etc/network/interfaces</span>
  - <span data-role="file">/etc/ntp.conf</span>
  - <span data-role="file">/etc/profile.d/xivo\_uuid.sh</span>
  - <span data-role="file">/etc/resolv.conf</span>
  - <span data-role="file">/etc/ssl/</span>
  - <span data-role="file">/etc/systemd/</span>
  - <span data-role="file">/etc/wanpipe/</span>
  - <span data-role="file">/etc/wazo-auth/</span>
  - <span data-role="file">/etc/wazo-call-logd/</span>
  - <span data-role="file">/etc/wazo-calld/</span>
  - <span data-role="file">/etc/wazo-chatd/</span>
  - <span data-role="file">/etc/wazo-dird/</span>
  - <span data-role="file">/etc/wazo-plugind/</span>
  - <span data-role="file">/etc/wazo-webhookd/</span>
  - <span data-role="file">/etc/xivo-agentd/</span>
  - <span data-role="file">/etc/xivo-agid/</span>
  - <span data-role="file">/etc/xivo-amid/</span>
  - <span data-role="file">/etc/xivo-confd/</span>
  - <span data-role="file">/etc/xivo-confgend-client/</span>
  - <span data-role="file">/etc/xivo-dird-phoned/</span>
  - <span data-role="file">/etc/xivo-dxtora/</span>
  - <span data-role="file">/etc/xivo-purge-db/</span>
  - <span data-role="file">/etc/xivo-websocketd/</span>
  - <span data-role="file">/etc/xivo/</span>
  - <span data-role="file">/root/.config/wazo-auth-cli/</span>
  - <span data-role="file">/usr/local/bin/</span>
  - <span data-role="file">/usr/local/sbin/</span>
  - <span data-role="file">/usr/share/xivo/XIVO-VERSION</span>
  - <span data-role="file">/var/lib/asterisk/</span>
  - <span data-role="file">/var/lib/consul/</span>
  - <span data-role="file">/var/lib/wazo-auth-keys/</span>
  - <span data-role="file">/var/lib/xivo-provd/</span>
  - <span data-role="file">/var/lib/xivo/</span>
  - <span data-role="file">/var/log/asterisk/</span>
  - <span data-role="file">/var/spool/asterisk/</span>
  - <span data-role="file">/var/spool/cron/crontabs/</span>

The following files/folders are excluded from this
        backup:

  - folders:
      - <span data-role="file">/var/lib/consul/checks</span>
      - <span data-role="file">/var/lib/consul/raft</span>
      - <span data-role="file">/var/lib/consul/serf</span>
      - <span data-role="file">/var/lib/consul/services</span>
      - <span data-role="file">/var/lib/xivo-provd/plugins/\*/var/cache/\*</span>
      - <span data-role="file">/var/spool/asterisk/monitor/</span>
      - <span data-role="file">/var/spool/asterisk/meetme/</span>
  - files
      - <span data-role="file">/var/lib/xivo-provd/plugins/xivo-polycom\*/var/tftpboot/\*.ld</span>
  - log files, coredump files
  - audio recordings
  - and, files greater than 10 MiB or folders containing more than 100
    files if they belong to one of these folders:
      - <span data-role="file">/var/lib/xivo/sounds/</span>
      - <span data-role="file">/var/lib/asterisk/sounds/custom/</span>
      - <span data-role="file">/var/lib/asterisk/moh/</span>
      - <span data-role="file">/var/spool/asterisk/voicemail/</span>
      - <span data-role="file">/var/spool/asterisk/monitor/</span>

### Database

The following databases from PostgreSQL are backed up:

  - `asterisk`: all the configuration done via the web interface
    (exceptions: High Availability, Provisioning, Certificates)

## Creating backup files manually

<div class="warning">

<div class="admonition-title">

Warning

</div>

A backup file may take a lot of space on the disk. You should check the
free space on the partition before creating one.

</div>

### Database

You can manually create a *database* backup file named
<span data-role="file">db-manual.tgz</span> in
<span data-role="file">/var/tmp</span> by issuing the following
commands:

    xivo-backup db /var/tmp/db-manual

### Files

You can manually create a *data* backup file named
<span data-role="file">data-manual.tgz</span> in
<span data-role="file">/var/tmp</span> by issuing the following
commands:

    xivo-backup data /var/tmp/data-manual

# Restore

## Introduction

A backup of both the configuration files and the database used by a Wazo
installation is done automatically every day. These backups are created
in the <span data-role="file">/var/backups/xivo</span> directory and are
kept for 7 days.

## Limitations

  - You must restore a backup on the **same version** of Wazo that was
    backed up (though the architecture -- `i386` or `amd64` -- may
    differ)
  - You must restore a backup on a machine with the **same hostname and
    IP address**

## Before Restoring the System

<div class="warning">

<div class="admonition-title">

Warning

</div>

Before restoring a Wazo on a fresh install you have to setup Wazo using
the wizard.

</div>

Stop monit and all the Wazo services:

    wazo-service stop

## Restoring System Files

System files are stored in the data.tgz file located in the
<span data-role="file">/var/backups/xivo</span> directory.

This file contains for example, voicemail files, musics, voice guides,
phone sets firmwares, provisioning server configuration database.

To restore the file :

    tar xvfp /var/backups/xivo/data.tgz -C /

Once the database and files have been restored, you can
<span data-role="ref">finalize the restore \<after\_restore\></span>

## Restoring the Database

<div class="warning">

<div class="admonition-title">

Warning

</div>

  - This will destroy all the current data in your database.
  - You have to check the free space on your system partition before
    extracting the backups.
  - If restoring Wazo \>= 18.01 on a different machine, you should not
    restore the system configuration, because of network interface names
    that would change. See
    <span data-role="ref">restore\_keep\_system\_config</span>.

</div>

Database backups are created as <span data-role="file">db.tgz</span>
files in the <span data-role="file">/var/backups/xivo</span> directory.
These tarballs contains a dump of the database used in Wazo.

In this example, we'll restore the database from a backup file named
<span data-role="file">db.tgz</span> placed in the home directory of
root.

First, extract the content of the <span data-role="file">db.tgz</span>
file into the <span data-role="file">/var/tmp</span> directory and go
inside the newly created directory:

    tar xvf db.tgz -C /var/tmp
    cd /var/tmp/pg-backup

Drop the asterisk database and restore it with the one from the backup:

    sudo -u postgres dropdb asterisk
    sudo -u postgres pg_restore -C -d postgres asterisk-*.dump

Once the database and files have been restored, you can
<span data-role="ref">finalize the restore \<after\_restore\></span>

### Troubleshooting

When restoring the database, if you encounter problems related to the
system locale, see
<span data-role="ref">postgresql\_localization\_errors</span>.

## Alternative: Restoring and Keeping System Configuration

System configuration like network interfaces is stored in the database.
It is possible to keep this configuration and only restore Wazo data.

Rename the asterisk database to
    asterisk\_previous:

    sudo -u postgres psql -c 'ALTER DATABASE asterisk RENAME TO asterisk_previous'

Restore the asterisk database from the backup:

    sudo -u postgres pg_restore -C -d postgres asterisk-*.dump

Restore the system configuration tables from the asterisk\_previous
database:

    sudo -u postgres pg_dump -c -t dhcp -t netiface -t resolvconf asterisk_previous | sudo -u postgres psql asterisk

Drop the asterisk\_previous database:

    sudo -u postgres dropdb asterisk_previous

<div class="warning">

<div class="admonition-title">

Warning

</div>

Restoring the data.tgz file also restores system files such as host
hostname, network interfaces, etc. You will need to reapply the network
configuration if you restore the data.tgz file.

</div>

Once the database and files have been restored, you can
<span data-role="ref">finalize the restore \<after\_restore\></span>

## After Restoring The System

1.  Restore the server
        UUID:
    
        XIVO_UUID=$(sudo -u postgres psql -d asterisk -tA -c 'select uuid from infos')
        echo "export XIVO_UUID=$XIVO_UUID" > /etc/profile.d/xivo_uuid.sh
    
    Then edit <span data-role="file">/etc/systemd/system.conf</span> to
    update `XIVO_UUID` in `DefaultEnvironment`

2.  You may reboot the system, or execute the following steps.

3.  Update systemd runtime configuration:
    
        source /etc/profile.d/xivo_uuid.sh
        systemctl set-environment XIVO_UUID=$XIVO_UUID
        systemctl daemon-reload

4.  Restart the services you stopped in the first step:
    
        wazo-service start
