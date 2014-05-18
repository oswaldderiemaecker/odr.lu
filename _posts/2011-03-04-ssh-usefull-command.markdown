---
layout: post
title:  "SSH Usefull command"
date:   2011-03-04 21:47:09
categories: linux
---

## SSH connection through host in the middle

``` bash
ssh -t reachable_host ssh unreachable_host
```

Unreachable_host is unavailable from local network, but it’s available from reachable_host’s network. This command creates a connection to unreachable_host through “hidden” connection to reachable_host.

## Run any GUI program remotely

``` bash
ssh -fX <user>@<host> <program>
```

The SSH server configuration requires:

``` bash
X11Forwarding yes
Compression delayed
```

## Run complex remote shell cmds over ssh, without escaping quotes

``` bash
ssh -MNf <user>@<host>
```

Create a persistent SSH connection to the host in the background. Combine this with settings in your ~/.ssh/config:

``` bash
Host host
ControlPath ~/.ssh/maste
```

## Run complex remote shell cmds over ssh, without escaping quotes

``` bash
ssh host -l user $(<cmd.txt)
```

Much simpler method. More portable version: ssh host -l user “`cat cmd.txt`”

## Remove a line in a text file. Useful to fix “ssh host key change” warnings

``` bash
sed -i 8d ~/.ssh/known_hosts
```

## Resume scp of a big file

``` bash
rsync –partial –progress –rsh=ssh $file_source $user@$host:$destination_file
```

It can resume a failed secure copy ( usefull when you transfer big files like db dumps through vpn ) using rsync.
It requires rsync installed in both hosts.

``` bash
rsync –partial –progress –rsh=ssh $file_source $user@$host:$destination_file local -> remote
```

or

``` bash
rsync –partial –progress –rsh=ssh $user@$host:$remote_file $destination_file remote -> local
```

## Have an ssh session open forever

``` bash
autossh -M50000 -t server.example.com ‘screen -raAd mysession’
```

Open a ssh session opened forever, great on laptops losing Internet connectivity when switching WIFI spots.

## Mount folder/filesystem through SSH

``` bash
sshfs name@server:/path/to/folder /path/to/mount/point
```

Install SSHFS from http://fuse.sourceforge.net/sshfs.html
Will allow you to mount a folder security over a network.
