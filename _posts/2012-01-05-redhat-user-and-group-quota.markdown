---
layout: post
title:  "User and Group quota"
date:   2012-01-05 21:47:09
categories: mysql
---

## Enable Quota in /etc/fstab

```bash
/dev/VolGroup00/LogVol02 /home ext3 defaults,usrquota,grpquota 1 2
```

## Remount the FileSystem

```bash
mount -o remount /home
```

## Creating the Quota Database Files

```bash
quotacheck -cug /home
```

The -c option specifies that the quota files should be created for each file system with quotas enabled, the -u option specifies to check for user quotas, and the -g option specifies to check for group quotas.

After the files are created, run the following command to generate the table of current disk usage per file system with quotas enabled:

```bash
quotacheck -avug
```

The options used are as follows:

a — Check all quota-enabled, locally-mounted file systems

v — Display verbose status information as the quota check proceeds

u — Check user disk quota information

g — Check group disk quota information

## Assigning Quotas per User

To configure the quota for a user, as root in a shell prompt, execute the command:

```bash
edquota username
```

Perform this step for each user who needs a quota. For example, if a quota is enabled in /etc/fstab for the /home partition (/dev/VolGroup00/LogVol02 in the example below) and the command edquota testuser is executed, the following is shown in the editor configured as the default for the system:

Disk quotas for user testuser (uid 501): 
Filesystem blocks soft hard inodes soft hard 
/dev/VolGroup00/LogVol02 440436 0 0 37418 0 0

1. Blocks - Number of blocks (Kilobytes) the user is currently using.
2. Soft - The soft limit for the amount of blocks (Kilobytes) the user can use. The soft limit can be surpassed, up to the hard limit, but only for a specified grace period - which is also configurable.
3. Hard - The hard limit for the amount of blocks (Kilobytes) the user can use. The hard limit is the maximum limit that can be used by the user and cannot be surpassed.
4. Inodes - Number of inodes the user is currently using. An inode is used for every file or directory on a linux filesystem. Limiting the number of inodes is usually not as important to most system administrators as limiting block usage - however, it isn't a bad idea to limit a user's inode usage too, as a filesystem can run out of inodes (which will deny the server from creating any new files or directories).
5. Soft - The soft limit for the amount of inodes the user can use. The soft limit can be surpassed, up to the hard limit, but only for a specified grace period.
6. Hard - The hard limit for the amount of inodes the user can use. The hard limit is the maximum limit that can be used by the user and cannot be surpassed.

To verify that the quota for the user has been set, use the command:

```bash
quota testuser
```

## Assigning Quotas per Group

```bash
edquota -g devel
```

This command displays the existing quota for the group in the text editor:

Disk quotas for group devel (gid 505): 
Filesystem blocks soft hard inodes soft hard 
/dev/VolGroup00/LogVol02 440400 0 0 37418 0 0 
          
Modify the limits, then save the file.

To verify that the group quota has been set, use the command:

```bash
quota -g devel
```

## Setting the Grace Period for Soft Limits

If soft limits are set for a given quota (whether inode or block and for either users or groups) the grace period, or amount of time a soft limit can be exceeded, should be set with the command:

```bash
edquota -t
```

While other edquota commands operate on a particular user's or group's quota, the -t option operates on every filesystem with quotas enabled.

## Enabling and Disabling

To turn all user and group quotas off, use the following command:

```bash
quotaoff -vaug
```

To enable quotas again, use the quotaon command with the same options.

For example, to enable user and group quotas for all file systems, use the following command:

```bash
quotaon -vaug
```

To enable quotas for a specific file system, such as /home, use the following command:

```bash
quotaon -vug /home
```

If neither the -u or -g options are specified, only the user quotas are enabled. If only -g is specified, only group quotas are enabled.

## Reporting on Disk Quotas

To view the disk usage report for all (option -a) quota-enabled file systems, use the command:

```bash
repquota -a
```
 
## Updating Quotas
Add

```bash
quotacheck -avug
```

In /etc/cron.daily/quotacheck
