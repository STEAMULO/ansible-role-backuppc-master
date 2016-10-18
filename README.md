BackupPC Master Ansible role
=====================

This role installs and configures Backuppc. It works on Debian Jessie. It can work on Ubuntu or other Debian-based systems. But no direct support will be added (PR accepted).

Requirements
------------

None.

Role Variables
--------------

### Role var

- `backuppc_server_name`: fqdn of the backup server (Mandatory)
- `backuppc_ssh_key_bits`: set length of ssh key pair (optional, default 2048 by Ansible)
- `backuppc_hosts`: clients list to backup (see below)

### Client vars

Each client configuration override global configuration.

- `hostname`: (M) hostname of the host
- `state`: (O) absent or present (default)
- `include_files:`: (O) default files (directories) list to backup.
- `exclude_files:`: (O) default files (directories) list to exclude in backup
- `xfermethod`: (O) transfer method (rsync as default)
- `others`: (O) hash with specific key/value (usefull for custom directives)

(O): Optional (M): Mandatory


### Global configuration

You should [RTFM](http://backuppc.sourceforge.net/faq/BackupPC.html) for these variables:

- `backuppc_ServerPort`
- `backuppc_ServerMesgSecret`
- `backuppc_MaxBackups`
- `backuppc_MaxUserBackups`
- `backuppc_MaxBackupPCNightlyJobs`
- `backuppc_BackupPCNightlyPeriod`
- `backuppc_MaxOldLogFiles`
- `backuppc_FullPeriod`
- `backuppc_IncrPeriod`
- `backuppc_PingMaxMsec`
- `backuppc_RsyncClientCmd`
- `backuppc_RsyncClientRestoreCmd`
- `backuppc_BackupFilesOnly`
- `backuppc_BackupFilesExclude`
- `backuppc_XferLogLevel`

Notes
-----


Dependencies
------------


Example Playbook
----------------


- hosts: all
  become: true
  vars:
    backuppc_RsyncClientCmd: '$sshPath -q -x -l backuppc $host sudo $rsyncPath $argList+'
    backuppc_RsyncClientRestoreCmd: '$sshPath -q -x -l backuppc $host sudo $rsyncPath $argList+'

    backuppc_hosts:
        - hostname: "localhost"
          state: "present"
          include_files: []
          exclude_files: []
          xfermethod: 'tar'
          others:
            TarShareName:
                - '/etc/backuppc'
                - '/etc/apache2'
            TarClientCmd: '/usr/bin/env LC_ALL=C $tarPath -c -v -f - -C $shareName --totals'
            TarFullArgs: '$fileList'
            TarIncrArgs: '--newer=$incrDate $fileList'
  roles:
    - ansible-role-backuppc-master

License
-------

GPLv2

Author Information
------------------

Steamulo - www.steamulo.com

Forked from [@hanx_hx](https://twitter.com/hanxhx_)
