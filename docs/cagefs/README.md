# CageFS


CageFS is a virtualized file system and a set of tools to contain each user in its own 'cage'. Each customer will have its own fully functional CageFS, with all the system files, tools, etc.

The benefits of CageFS are:

·        Only safe binaries are available to user
·        User will not see any other users, and would have no way to detect presence of other users & their user names on the server
·        User will not be able to see server configuration files, such as Apache config files.
·        User's will have limited view of /proc file system, and will not be able to see other' users processes

At the same time, user's environment will be fully functional, and user should not feel in any way restricted. No adjustments to user's scripts are needed. CageFS will cage any scripts execution done via:
<span class="notranslate"> </span>
·        Apache (suexec, suPHP, mod_fcgid, mod_fastcgi)
·        LiteSpeed Web Server
·        Cron Jobs
·        SSH
·        Any other <span class="notranslate"> PAM </span> enabled service





## Installation


Minimum Requirements:

kernel: CL5 with lve0.8.54 or later, CL6 with lve1.2.17.1 or later, CL7.
7GB of disk space.

Depending on your setup, and number of users, you might also need:
Up to 8MB per customer in /var directory (to store custom /etc directory)
5GB to 20GB in /usr/share directory (to store safe skeleton of a filesystem)





To install CageFS:

```
$ yum install cagefs$ /usr/sbin/cagefsctl --init
```

That last command will create skeleton directory that might be around 7GB in size. If you don't have enough disk space in /usr/share, use following commands to have cagefs-skeleton being placed in a different location:

```
$ mkdir /home/cagefs-skeleton$ ln -s /home/cagefs-skeleton /usr/share/cagefs-skeleton
```





CageFS will automatically detect and configure all necessary files for:
cPanel
Plesk
DirectAdmin
ISPmanager
Interworx
MySQL
PostgreSQL
LiteSpeed

Web interface to manage CageFS is available for cPanel, Plesk 10+, DirectAdmin, ISPmanager & Interworx. Command line tool would need to be used for other control panels.

Once you initialized the template you can start enabling users. By default CageFS is disabled for all users.

Starting from **cagefs-6.1-27**  _fs.proc_can_see_other_uid_ will be migrated (one time) from _/etc/sysctl.conf_ into _/etc/sysctl.d/90-cloudlinux.conf_ . If this variable is not set in either file, it will default to 0.

It is strongly advised against setting this variable in _90-cloudlinux.conf_ . Define it in _/etc/sysctl.conf_ or in some other config file with an index number greater than _90-cloudlinux.conf_ , e.g. _/etc/sysctl.d/95-custom.conf_ .

You can find more information on _fs.proc_can_see_other_uid_ automatic migration in [Kernel Config Variables](/kernel_settings/#kernel-config-variables) .


## Unistalling CageFS


To uninstall CageFS, start by disabling and removing all directories:

<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --remove-all
```


That command will: Disable CageFS for all customers, unmount CageFS for all users, removes <span class="notranslate"> /usr/share/cagefs-skeleton & /var/cagefs </span> directories. It will not remove /etc/cagefs directory.

Remove CageFS RPM:

<span class="notranslate"> </span>
```
$ yum remove cagefs
```



## Managing Users


CageFS provides for two modes of operations:

1.        Enabled for all, except those that are disabled.
2.        Disabled for all, except those that are enabled.

Mode #1 is convenient for production operation, where you want all new users to automatically be added to CageFS.
Mode #2 is convenient while you test CageFS, as it allows you to enable it on one by one for your customers.

To start using CageFS you have to select one of the mode of operations:
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --enable-all
```

or
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --disable-all
```

or
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --toggle-mode
```

That will switch the operation mode, preserving current disabled/enabled users.

To enable individual user do:
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --enable [username]
```

To disable individual user:
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --disable [username]
```

To  list all enabled users:
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --list-enabled
```

To list all disabled users:
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --list-disabled
```

To see current mode of operation:
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --display-user-mode
```


## Command-line Tools


_cagefsctl _ is used to manage CageFS. It allows initializing and updating CageFS, as well as enabling/disabling CageFS for individual users.

Use the following syntax to manage CageFS:
<span class="notranslate"> </span>
`/usr/sbin/cagefsctl [OPTIONS]`


`Options:`

` ` <span class="notranslate"> -i | --init </span> `                 : initialize CageFS (create CageFS if it does not exist)`
` -r | --reinit               : reinitialize CageFS (make backup and recreate CageFS)`
` ` <span class="notranslate"> -u | --update </span> `               : update files in CageFS (add new and modified files to CageFS,`
`                               remove unneeded files)`
` ` <span class="notranslate"> -f | --force </span> `                : recreate CageFS (do not make backup, overwrite existing files)`
` ` <span class="notranslate"> -d | --dont-clean </span> `           : do not delete any files from skeleton`
`                                                                                      : (use with ` <span class="notranslate"> --update </span> ` option)`
` ` <span class="notranslate"> -k | --hardlink </span> `             : use hardlinks if possible`
`      ` <span class="notranslate"> --create-mp </span> `            : Creates /etc/cagefs/cagefs.mp file`
`      ` <span class="notranslate"> --mount-skel </span> `           : mount CageFS skeleton directory`
`      ` <span class="notranslate"> --unmount-skel </span> `         : unmount CageFS skeleton directory`
`      ` <span class="notranslate"> --remove-all </span> `           : disable CageFS, remove templates and /var/cagefs directory`
`      ` <span class="notranslate"> --sanity-check </span> `         : perform basic self-diagnistics of common cagefs-related issues`
`                                                                                      : (mostly useful for support)`
`      ` <span class="notranslate"> --addrpm </span> `               : add rpm-packages in CageFS (run ` <span class="notranslate"> "cagefsctl --update" </span>
`                                                                                      : in order to apply changes)`
`      ` <span class="notranslate"> --delrpm </span> `               : remove rpm-packages from CageFS (run ` <span class="notranslate"> "cagefsctl --update" </span>
`                                                                                      : in order to apply changes)`
`      ` <span class="notranslate"> --list-rpm </span> `             : list rpm-packages that are installed in CageFS`
` ` <span class="notranslate"> -e | --enter </span> `                : enter into user's CageFS as root`
`      ` <span class="notranslate"> --update-list </span> `          : update specified files only (paths are read from stdin)`
`      ` <span class="notranslate"> --update-etc </span> `           : update /etc directory of all or specified users`
`      ` <span class="notranslate"> --set-update-period </span> `    : set min period of update of CageFS in days (default = 1 day)`
`      ` <span class="notranslate"> --force-update </span> `         : force update of CageFS (ignore period of update)`
`      ` <span class="notranslate"> --force-update-etc </span> `     : force update of /etc directories for users in CageFS`
`      ` <span class="notranslate"> --reconfigure-cagefs </span> `   : configure CageFS integration with other software (control panels,`
`                               database servers, etc)`

Use the following syntax to manage users:
<span class="notranslate"> </span>
`/usr/sbin/cagefsctl [OPTIONS] username [more usernames]`


`Options:`

` ` <span class="notranslate"> -m | --remount </span> `           : remount specified user(s)`
` ` <span class="notranslate"> -M | --remount-all </span> `       : remount CageFS skeleton directory and all users`
`                            (use this each time you have changed cagefs.mp file)`
` ` <span class="notranslate"> -w | --unmount </span> `           : unmount specified user(s)`
`    ` <span class="notranslate"> | --unmount-dir </span> `       : unmount specified dir for all users`
` ` <span class="notranslate"> -W | --unmount-all </span> `       : unmount CageFS skeleton directory and all users`
` ` <span class="notranslate"> -l | --list </span> `              : list users that entered in CageFS`
`      ` <span class="notranslate"> --list-logged-in </span> `    : list users that entered in CageFS via SSH`
`      ` <span class="notranslate"> --enable </span> `            : enable CageFS for the user`
`      ` <span class="notranslate"> --disable </span> `           : disable CageFS for the user`
`      ` <span class="notranslate"> --enable-all </span> `        : enable all users, except specified in /etc/cagefs/users.disabled`
`      ` <span class="notranslate"> --disable-all </span> `       : disable all users, except specified in /etc/cagefs/users.enabled`
`      ` <span class="notranslate"> --display-user-mode </span> ` : display current mode (` <span class="notranslate"> "Enable All" </span> ` or ` <span class="notranslate"> "Disable All" </span> `)`
`      ` <span class="notranslate"> --toggle-mode </span> `       : toggle mode saving current lists of users`
`                            (lists of enabled and disabled users remain unchanged)`
`      ` <span class="notranslate"> --list-enabled </span> `      : list enabled users`
`      ` <span class="notranslate"> --list-disabled </span> `     : list disabled users`
`      ` <span class="notranslate"> --user-status </span> `       : print status of specified user (enabled or disabled)`
`      ` <span class="notranslate"> --getprefix </span> `         : display prefix for user`

<span class="notranslate"> `PHP Selector`  related options: </span>

`      ` <span class="notranslate"> --setup-cl-selector </span> `         : setup ` <span class="notranslate"> PHP Selector </span> ` or register new alt-php versions`
`      ` <span class="notranslate"> --remove-cls-selector </span> `       : unregister alt-php versions, switch users to default`
`                                                                                           : php version when needed`
`      ` <span class="notranslate"> --rebuild-alt-php-ini </span> `       : rebuild alt_php.ini file for specified users`
`                                                                                           : (or all users if none specified)`
`      ` <span class="notranslate"> --validate-alt-php-ini </span> `      : same as ` <span class="notranslate"> --rebuild-alt-php-ini </span>
`                                                                                           : but also validates alt_php.ini options `
`      ` <span class="notranslate"> --cl-selector-reset-versions </span> `: reset php version for specifed users to default`
`                                                                                           : (or all users if none specified)`
`      ` <span class="notranslate"> --cl-selector-reset-modules </span> ` : reset php modules (extensions) for specific users`
`                                                                                           : to defaults (or all users if none specified)`
`      ` <span class="notranslate"> --create-virt-mp </span> `            : create virtual mount points for the user`
`      ` <span class="notranslate"> --create-virt-mp-all </span> `        : create virtual mount points for all users`
`      ` <span class="notranslate"> --remount-virtmp </span> `            : create virtual mount points and remount user`
`      ` <span class="notranslate"> --apply-global-php-ini </span> `      : use with 0, 1 or 2 arguments from the list: error_log,`
`                                                                                           : date.timezone without arguments applies`
`                                                                                           : all global php options including two above`

Common options:


`      ` <span class="notranslate"> --disable-cagefs </span> `       : disable CageFS`
`      ` <span class="notranslate"> --cagefs-status </span> `        : print CageFS status (` <span class="notranslate"> enabled </span> ` or ` <span class="notranslate"> disabled </span> `)`
`      ` <span class="notranslate"> --set-min-uid </span> `          : Set min ` <span class="notranslate"> UID </span>
`      ` <span class="notranslate"> --get-min-uid </span> `          : Display current MIN_UID setting`
`      ` <span class="notranslate"> --print-suids </span> `          : Print list of ` <span class="notranslate"> SUID </span> ` and SGID programs in skeleton`
`      ` <span class="notranslate"> --do-not-ask </span> `           : assume ` <span class="notranslate"> "yes" </span> ` in all queries`
`                                                                                      : (should be the first option in command)`
`      ` <span class="notranslate"> --clean-var-cagefs </span> `     : clean /var/cagefs directory (remove data of non-existent users)`
`      ` <span class="notranslate"> --set-tmpwatch </span> `         : set tmpwatch command and parameters`
`                                                                                      : (save to /etc/cagefs/cagefs.ini file)`
`      ` <span class="notranslate"> --tmpwatch </span> `             : execute tmpwatch (remove outdated files in tmp directories`
`                                                                                      : in CageFS for all users)`
`      ` <span class="notranslate"> --toggle-plugin </span> `        : disable/enable CageFS plugin`
` ` <span class="notranslate"> -v | --verbose </span> `              : verbose output`
`      ` <span class="notranslate"> --wait-lock </span> `            : wait for end of execution of other cagefsctl processes`
`                                                                                      : (when needed) before execution of the command`
` ` <span class="notranslate"> -h | --help </span> `                 : this message`



## Running Command Inside CageFS


<span class="notranslate"> _[lve-wrappers 0.6-1+]_ </span>

Sometimes you will need to execute a command as user inside CageFS.

If a user has shell enabled - you can simply use:
<span class="notranslate"> </span>
```
$ /bin/su - $USERNAME  -c "_command_"
```

Yet, if user has shell disabled, it wouldn't work. To solve this issue, we have added command:

<span class="notranslate"> </span>
```
$ /sbin/cagefs_enter_user $USERNAME "_command_"
```


If you disable CageFS for a user,  then <span class="notranslate"> `cagefs_enter` </span> will be executed without <span class="notranslate"> proxyexec </span> .

You can forcibly disable <span class="notranslate"> `cagefs_enter` </span> start via <span class="notranslate"> proxyexec </span> for all users (regardless if CageFS is enabled or disabled) by specifying the parameter _cagefs_enter_proxied=0_ in _/etc/sysconfig/cloudlinux_ .

_/bin/cagefs_enter.proxied_ can be executed instead of _/bin/cagefs_enter_ to enter CageFS without <span class="notranslate"> proxyexec </span> . Note that starting <span class="notranslate"> `cagefs_enter` </span> via <span class="notranslate"> proxyexec </span> is necessary to enable sending local notification messages to users with enabled CageFS. <span class="notranslate"> `cagefs_enter` </span> is executed via <span class="notranslate"> proxyexec </span> by default.


## Sanity Check


<span class="notranslate"> _[ CageFS 6.0-34+]_ </span>

CageFS <span class="notranslate"> `--sanity-check` </span> utility allows to check CageFS configuration consistency, so that an administrator can save the time investigating issues with CageFS and ensure that custom configuration is correct.

To start, run the command:
<span class="notranslate"> </span>
```
cagefsctl --sanity-check
```

At the moment 7 types of check are implemented:

_Check cagefs mount points exists_ - reads `cagefs.mp` file and verifies if the directories specified in it really exist on the disk. To learn more visit [https://docs.cloudlinux.com/index.html?mount_points.html](https://docs.cloudlinux.com/index.html?mount_points.html) and [https://docs.cloudlinux.com/index.html?split_by_username.html](https://docs.cloudlinux.com/index.html?split_by_username.html)

_Check cagefs users.enabled is directory_ - ensures that if `/etc/cagefs/users.enabled` exists, then it is a directory, not a file (if it is recognized as a file, then it would cause a breakdown).

_Check cagefs users.disabled is directory_ - ensures that if `/etc/cagefs/users.disabled` exists, then it is a directory, not a file (if it is recognized as a file, then it would cause a breakdown).

_Check cagefs disable.etcfs exists_ - checks if `/etc/cagefs/etc.safe/disable.etcfs` exists.

_Check cagefs users can enter cagefs_ - chooses two users in the system with enabled CageFS (the first and the second ones in the unsorted list) and tries to log in to CageFS under their credentials and see what happens. It runs <span class="notranslate"> `su -l "$USER" -s /bin/bash -c "whoami"` </span> and compares the output with the <span class="notranslate"> $USER </span> and <span class="notranslate"> su </span> command retcode estimation.





_Check cagefs proxy commands configs are parsable_ - tries to load `/etc/cagefs/*.proxy.commands` files and parse them to check the syntax. In case of any parsing error the test will fail. To learn more, visit [https://docs.cloudlinux.com/index.html?executing_by_proxy.html](https://docs.cloudlinux.com/index.html?executing_by_proxy.html) .

_Check cagefs virt.mp files syntax_ - reads all `/var/cagefs/*/*/virt.mp` files (if any) and checks their syntax validity. At the moment there are only two checks of the syntax: the file is not empty if it exists, and the file is not starting with the sub directory definitions (with @). To learn more, visit [https://docs.cloudlinux.com/index.html?per_user_virtual_mount_points.html](https://docs.cloudlinux.com/index.html?per_user_virtual_mount_points.html)

_Check MultiPHP system default PHP version – _ checks that MultiPHP system default PHP version is **NOT** Alt-PHP. That means PHP Selector should work properly. If MultiPHP system default PHP version is Alt-PHP, PHP Selector does not work and should be disabled. To learn more on how to disable PHP Selector, visit [https://docs.cloudlinux.com/cpanel_lve_manager.html](https://docs.cloudlinux.com/cpanel_lve_manager.html) 

Possible results of the checks:

 <span class="notranslate"> OK </span> - the check succeeded.

 <span class="notranslate"> FAILED </span> - the check revealed a problem.

 <span class="notranslate"> SKIPPED </span> - the check was skipped as it made no sense in such environment (e.g. wrong control panel) or can not be performed for some reason (e.g no users with enabled CageFS found). The actual result does not mean that a problem exists and can be considered as positive.

 <span class="notranslate"> INTERNAL_TEST_ERROR </span> - the check failed because of a problem inside the checker itself. Must be reported to the developers.

In case if at least one of the checks resulted neither <span class="notranslate"> OK </span> nor <span class="notranslate"> SKIPPED </span> then the checker will end with ret code >0.


## CageFS Quirks


Due to the nature of CageFS, some options will not work as before or will require some changes:

lastlog will not work ( _/var/log/lastlog_ ).
PHP will load php.ini from _/usr/selector/php.ini._ That file is actually a link to a real php.ini file from your system. So the same php.ini will be loaded in the end.
You have to run `cagefsctl --update` any time you have modified php.ini, or you want to get new/updated software inside CageFS.
CageFS installation changes jailshell to regular bash on cPanel - [read why](http://kb.cloudlinux.com/2015/11/why-cagefs-installation-change-jailshell-to-regular-bash-on-cpanel/) .



## Configuration


[File System Templates](/cagefs/#file-system-templates)

[Excluding Files](/cagefs/#excluding-files)

[Excluding Users](/cagefs/#excluding-users)

[Mount Points](/cagefs/#mount-points)

[Per user virtual mount points](/cagefs/#per-user-virtual-mount-points)

[Split by Username](/cagefs/#split-by-username)

[Base Home Directory](/cagefs/#base-home-directory)

[PostgreSQL support](/cagefs/#postgresql-support)

[PAM Configuration](/cagefs/#pam-configuration)

[Executing By Proxy](/cagefs/#executing-by-proxy)

[Custom /etc directory](/cagefs/#custom-etc-files-per-customer)

[Moving cagefs-skeleton directory](/cagefs/#moving-cagefs-skeleton-directory)

[Moving /var/cagefs directory](/cagefs/#moving-var-cagefs-directory)

[TMP directories](/cagefs/#tmp-directories)

[Syslog](/cagefs/#syslog)

[Excluding mount points](/cagefs/#excluding-mount-points)


### File System Templates


CageFS creates a filesystem template in <span class="notranslate"> /usr/share/cagefs-skeleton </span> directory. CageFS template will be mounted for each customer.  The template is created by running:
<span class="notranslate"> </span>
```
# /usr/sbin/cagefsctl --init
```


To update the template, you should run:
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --update
```


The behavior of the commands (and the files copied into <span class="notranslate"> /usr/share/cagefs-skeleton </span> directory) depends on the configuration files in /etc/cagefs/conf.d
You can add additional files, users, groups and devices into CageFS template by adding .cfg file, and running:
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --update
```


To delete files from CageFS template, remove corresponding .cfg file, and run:
<span class="notranslate"> </span>
```
$ /usr/sbin/cagefsctl --update
```


Here is an example <span class="notranslate"> openssh-clients.cfg </span> file:
<span class="notranslate"> </span>
```
[openssh-clients]
```
```
comment=OpenSSH Clients
```
```
paths=/etc/ssh/ssh_config, /bin/hostname, /usr/bin/scp, /usr/bin/sftp, /usr/bin/slogin, /usr/bin/ssh, /usr/bin/ssh-add, /usr/bin/ssh-agent, /usr/bin/ssh-copy-id, /usr/bin/.ssh.hmac, /usr/bin/ssh-keyscan, /usr/libexec/openssh/sftp-server, /etc/environment, /etc/security/pam_env.conf
```
```
devices=/dev/ptmx
```


Example <span class="notranslate"> mail.cfg </span> file:
<span class="notranslate"> </span>
```
[mail]
```
```
comment=Mail tools
```
```
paths=/bin/mail, /etc/aliases.db, /etc/mail, /etc/mailcap, /etc/mail.rc, /etc/mime.types, /etc/pam.d/smtp.sendmail, /etc/rc.d/init.d/sendmail, /etc/smrsh, /etc/sysconfig/sendmail, /usr/bin/hoststat, /usr/bin/Mail, /usr/bin/mailq.sendmail, /usr/bin/makemap, /usr/bin/newaliases.sendmail, /usr/bin/purgestat, /usr/bin/rmail.sendmail, /usr/lib64/sasl2/Sendmail.conf, /usr/lib/mail.help, /usr/lib/mail.tildehelp, /usr/lib/sendmail.sendmail, /usr/sbin/mailstats, /usr/sbin/makemap, /usr/sbin/praliases, /usr/sbin/sendmail.sendmail, /usr/sbin/smrsh, /var/log/mail, /var/spool/clientmqueue, /var/spool/mqueue
```
```
users=smmsp
```
```
groups=smmsp
```


There is an easy way to add/delete files from particular <span class="notranslate"> RPM </span> s into CageFS. That can be done by using <span class="notranslate"> --addrpm and --delrpm </span> options in <span class="notranslate"> cagefsctl </span> . Like:
<span class="notranslate"> </span>
```
$ cagefsctl --addrpm ffmpeg$ cagefsctl --update
```







### Excluding Files


To exclude files and directories from CageFS, edit file:
<span class="notranslate"> </span>
`/etc/cagefs/custom.black.list`

And add files or directories in there, one per line.

Please do not edit /etc/cagefs/black.list file because it is replaced during the update of CageFS package.

### Excluding Users


To exclude users from CageFS, create a file (any name would work) inside _ _ `/etc/cagefs/exclude` ` ` folder, and list users that you would like to exclude from CageFS in that file.


### Mount Points


CageFS creates individual namespace for each user, making it impossible for users to see each other's files and creating high level of isolation. The way namespace is organized:

1. <span class="notranslate"> /usr/share/cagefs-skeleton </span> with safe files is created
2.        Any directory from the server that needs to be shared across all users is mounted into <span class="notranslate"> /usr/share/cagefs-skeleton </span>
a.        list of such directories is defined in /etc/cagefs/cagefs.mp
3. <span class="notranslate"> /var/cagefs/[prefix]/username </span> directory for each user. Prefix is defined as last two digits of user id. User id is taken from /etc/passwd file.
4.        Separate /etc directory is created and populated for each user inside <span class="notranslate"> /var/cagefs/[prefix]/username </span>
5.        /tmp directory is mounted for each user separately into <span class="notranslate"> ~username/.cagefs-tmp directory </span>
6.        Additional custom directories can be mounted for each user by defining them in /etc/cagefs/cagefs.mp
7. You can define custom directories per user using [virt.mp](/cagefs/#per-user-virtual-mount-points) files [CageFS 5.1 and higher]

To define individual custom directories in /etc/cagefs/cagefs.mp following format is used:
<span class="notranslate"> </span>
`@/full/path/to/directory,permission notation`


This is useful when you need to give each user its own copy of a particular system directory, like:
<span class="notranslate"> </span>
`@/var/run/screen,777`


Such entry would create separate <span class="notranslate"> /var/run/screen </span> for each user, with permissions set to 777

To modify mount points, edit /etc/cagefs/cagefs.mp. Here is an example of cagefs.mp:
<span class="notranslate"> </span>
```
/var/lib/mysql/var/lib/dav/var/www/cgi-bin/var/spool/dev/pts/usr/local/apache/domlogs/proc/opt@/var/spool/cron,700@/var/run/screen,777
```


If you want to change mount points, make sure you re-initialize mount points for all customers:

```
$ cagefsctl --remount-all
```

This command will kill all current processes and reset mount points.



#### Per user virtual mount points


**[CageFS 5.1 and higher]**

* _Please, see _ [Split by username](/cagefs/#split-by-username) _ feature, as it might be more simpler to implement in some cases._ 

Starting with CageFS 5.1 you can specify additional directories to be mounted inside user's CageFS. This can be specified for each user.
To specify virtual mount points for a user, create a file:
<span class="notranslate"> </span>
`/var/cagefs/[prefix]/[user]/virt.mp`


Inside that file, you can specify mount points in the following format:
<span class="notranslate"> </span>
```
virtdir1,mask@subdir1,mask@subdir2,maskvirdir2,mask@subdir3,mask@subdir4,mask>virtdir3,mask@subdir5,mask@subdir6,mask# comments
```


 <span class="notranslate"> mask </span> is always optional, if missing 0755 is used
Create virtual directory <span class="notranslate"> subdir/virtdir </span> , mount it to:
 <span class="notranslate"> skeleton jaildir/virtdir </span>
inside virtual directory, create directories <span class="notranslate"> subdir1, subdir2 </span>
mount <span class="notranslate"> virtdir1/subdir1 </span> to <span class="notranslate"> subdir/virtdir/subdir1 </span>
if <span class="notranslate"> virtdir </span> is started with >, create directory <span class="notranslate"> subdir/virtdir </span> , but don't mount it into <span class="notranslate"> jaildir </span> . This is needed for cases when <span class="notranslate"> virtdir </span> is inside home base dir.
if file /var/cagefs/[prefix]/[user]/virt.mp is missing -- no virt directories are loaded for that user.

Note that CageFS will automatically create those files for Plesk 10 & higher.

For example if we have plesk11.5 with two users <span class="notranslate"> cltest1 </span> , and <span class="notranslate"> cltest2 </span> :
<span class="notranslate"> </span>
```
cltest1 uid 10000 has domains: cltest1.com, cltest1-addon.com and sub1.cltest1.comcltest2 uid 10001 has domains: cltest2.com, cltest2-addon.com
```


In such case we would have file _/var/cagefs/00/cltest1/virt.mp_ :
<span class="notranslate"> </span>
```
>/var/www/vhosts/system,0755@cltest1-addon.com,0755@cltest1.com,0755[@sub1.cltest1.com,0755](mailto:@sub1.cltest1.com,0755)
```


and file: _ /var/cagefs/01/cltest2/virt.mp:_
<span class="notranslate"> </span>
```
>/var/www/vhosts/system@cltest2-addon.com[@cltest2.com](mailto:@cltest2.com)
```



#### Split by Username


**[CageFS 5.3.1+]**

Sometimes you might need to make sure that directory containing all users would show up as containing just that user inside CageFS. For example, if you have directory structure like:
<span class="notranslate"> </span>
```
/home/httpd/fcgi-bin/user1/home/httpd/fcgi-bin/user2
```


Then we can add the following line to /etc/cagefs/cagefs.mp file:
<span class="notranslate"> </span>
```
%/home/httpd/fcgi-bin
```


and execute:
<span class="notranslate"> </span>
```
cagefsctl --remount-all
```


After that each subdirectory of /home/httpd/fcgi-bin will be mounted for appropriate user in CageFS: <span class="notranslate"> /home/httpd/fcgi-bin/user1 </span> will be mounted for <span class="notranslate"> user1 </span> and <span class="notranslate"> /home/httpd/fcgi-bin/user2 </span> will be mounted for <span class="notranslate"> user2 </span> .




#### Mounting user’s home directory inside CageFS


CageFS 6.1-1 (and later) has improved mounting user’s home directory that is applied for users with home directories like <span class="notranslate"> _/home/user_  or  <span class="notranslate"> /homeN/user </span>   </span> (where <span class="notranslate"> N </span> = 0,1,..9).

In such case, earlier versions of CageFS always mount user’s home directory to <span class="notranslate"> _/home/user_ </span> and create symlink <span class="notranslate"> _/homeN -> /home_ </span> when needed, so user’s home directory can be accessed both via _ _ <span class="notranslate"> /home/user </span> and <span class="notranslate"> _/homeN/user_ </span> . This quirk leads to some rare incompatibilities between CageFS and other software (for example OpenCart), because real path of user’s home directory in CageFS and in real file system can differ.

New CageFS mounts user’s home directory in a way that its real path in CageFS is always the same as in real file system. Additionally, CageFS searches for symlinks like
<span class="notranslate"> _/homeX -> /homeY_ </span> and _ _ <span class="notranslate"> /homeX/user -> /homeY/user </span> in real system and creates such symlinks in user’s CageFS when found.

This new mounting mode is enabled by default. You can switch to old mounting mode by executing the following commands:
<span class="notranslate"> </span>
```
# touch /etc/cagefs/disable.home.dirs.search# cagefsctl --force-update# cagefsctl --remount-all
```





### Base Home Directory


If you have a custom setup where home directories are in a special format, like: <span class="notranslate"> /home/$USERNAME/data </span> , you can specify it using regular expressions. This is needed by CageFS to create safe home space for end user, where no other users are visible.

We will create empty: <span class="notranslate">  /var/cagefs/[prefix]/$USERNAME/home </span> , and then mount <span class="notranslate"> /home/$USERNAME </span> in that directory

To do that, create a file: <span class="notranslate"> /etc/cagefs/cagefs.base.home.dirs </span>

With content like:
<span class="notranslate"> </span>
```
^/home/^/var/www/users/
```


If there is no such file, the home directory without last component will be considered as a base dir, like with
<span class="notranslate"> /home/$USERNAME </span> we would create <span class="notranslate"> /var/cagefs/[prefix]/$USERNAME/home </span> , and then mount
<span class="notranslate"> /home/$USERNAME </span> in there

WIth <span class="notranslate"> /home/$USERNAME/data </span> as a home dir, we would assume that <span class="notranslate"> /home/$USERNAME </span> is the base directory, and we would create <span class="notranslate"> /var/cagefs/[prefix]/$USERNAME/home/$USERNAME/data </span> and then we would mount <span class="notranslate"> /home/$USERNAME/data </span> -- which would cause each user to see empty base directories for other users, exposing user names.


When you want to share directory structure among multiple users, you can add following line at the top of the <span class="notranslate"> /etc/cagefs/cagefs.base.home.dirs </span> file. This is useful on the systems that support sites with multiple users, with different home directories inside main 'site' directory.
<span class="notranslate"> </span>
```
mount_basedir=1
```


For example:

<span class="notranslate"> user1 </span> has home directory <span class="notranslate"> /var/www/vhosts/sitename.com/web_users/user1 </span>
<span class="notranslate"> user2 </span> has home directory <span class="notranslate"> /var/www/vhosts/sitename.com/web_users/user2 </span>
site admin has home directory <span class="notranslate"> /var/www/vhosts/sitename.com </span>

So, content of <span class="notranslate"> /etc/cagefs/cagefs.base.home.dirs </span> should be the following:

<span class="notranslate"> </span>
```
mount_basedir=1^/var/www/vhosts/[^/]+
```


Directory structure in <span class="notranslate"> /var/www/vhosts/sitename.com </span> will be mounted in CageFS for appropriate users.
Each user will have access to whole directory structure in <span class="notranslate"> /var/www/vhosts/sitename.com </span> (according to their permissions).




### PostgreSQL support


CloudLinux 7:

CageFS works with any PostgreSQL version installed from CloudLinux or CentOS repositories. PostgreSQL packages fo CloudLinux 7 come from upstream (CentOS) unmodified. PostgreSQL’s socket is located in /var/run/postgresql directory. This directory is mounted to CageFS by default (in cagefs-5.5-6.34 or later).

When PostgreSQL has been installed after CageFS install, please add line:
<span class="notranslate"> </span>
```
/var/run/postgresql
```


tо /etc/cagefs/cagefs.mp file and then execute:
<span class="notranslate"> </span>
```
cagefsctl --remount-all 
```


The steps above are enough to configure CageFS to work with PostgreSQL.

CloudLinux 6:

CageFS provides separate /tmp directory for each end user. Yet, PostgreSQL keeps its Unix domain socket inside server's main /tmp directory. In addition to that -- the location is hard coded inside PostgreSQL libraries.

To resolve the issue, CloudLinux provides version of PostgreSQL with modified start up script that can store PostgreSQL's socket in /var/run/postgres. The script automatically creates link from /tmp to that socket to prevent PostgreSQL dependent applications from breaking.

In addition to that, CageFS knows how to correctly link this socket inside end user's /tmp directory.

To enable PostgreSQL support in CageFS:

1.        Make sure you have updated to latest version of PostgreSQL.

2.        Edit file /etc/sysconfig/postgres, and uncomment <span class="notranslate"> SOCK_DIR </span> line.

3. Update CageFS configuration by running:
<span class="notranslate"> </span>
```
cagefsctl  --reconfigure-cagefs
```


4. Restart PostgreSQL by running:
<span class="notranslate"> </span>
```
$ service postgresql restart 
```


If you are using cPanel, you would also need to modify file: /etc/cron.daily/tmpwatch

And update line:
<span class="notranslate"> </span>
```
flags=-umc 
```


to:
<span class="notranslate"> </span>
```
flags=-umcl
```


to prevent symlink from being removed.


### PAM Configuration


CageFS depends on <span class="notranslate"> pam_lve </span> module tor PAM enabled services.When installed the module is automatically installed for following services:
<span class="notranslate"> </span>
·        sshd
·        crond
·        su

Following line is added to corresponding file in /etc/pam.d/:
<span class="notranslate"> </span>
```
session    required     pam_lve.so      100     1
```


Where 100 stands for minimum <span class="notranslate"> UID </span> to put into <span class="notranslate"> CageFS & LVE </span> , and 1 stands for CageFS enabled.


### Executing By Proxy


Some software has to run outside CageFS to be able to complete its job. This includes such programs as <span class="notranslate"> _passwd, sendmail_ ,  </span> etc.

CloudLInux uses <span class="notranslate"> proxyexec </span> technology to accomplish this goal. You can define any program to run outside CageFS, by specifying it in <span class="notranslate"> _/etc/cagefs/custom.proxy.commands_ </span> file. Do not edit existing <span class="notranslate"> _/etc/cagefs/proxy.commands_ </span> as it will be overwritten with next CageFS update.

Once program is defined, run this command to populate the skeleton:
<span class="notranslate"> </span>
```
$ cagefsctl --update
```

All the cPanel scripts located in <span class="notranslate"> _/usr/local/cpanel/cgi-sys_ / </span> that user might need to execute should be added to <span class="notranslate"> _proxy.commands_ v </span> .



The syntax of _ _ <span class="notranslate"> /etc/cagefs/*.proxy.commands </span> files is as follows:
<span class="notranslate"> </span>
_ALIAS:wrapper_name=username:path_to_executable_


Obligatory parameters are <span class="notranslate"> _ALIAS_ </span> and _ _ <span class="notranslate"> path_to_executable </span> .

 <span class="notranslate"> ALIAS </span> - any name which is unique within all <span class="notranslate"> /etc/cagefs/*.proxy.commands </span> files;

 <span class="notranslate"> wrapper_name </span> - the name of wrapper file, which is used as a replacement for executable file <span class="notranslate"> path_to_executable inside CageFS </span> . Wrapper files are located in <span class="notranslate"> /usr/share/cagefs/safeprograms </span> . If wrapper name is not specified, then default wrapper <span class="notranslate"> /usr/share/cagefs/safeprograms/cagefs.proxy.program </span> is used. Also, a reserved word <span class="notranslate"> “noproceed” </span> can be used, it will intend that wrapper is not in use (installed before) - applied for the commands with several <span class="notranslate"> ALIAS </span> , as in the example below.

 <span class="notranslate"> usernam </span> e - the name of a user on whose behalf <span class="notranslate"> path_to_executable </span> will run in the real system. If <span class="notranslate"> username </span> is not specified, then <span class="notranslate"> path_to_executable </span> will run on behalf the same user that is inside CageFS.

 <span class="notranslate"> path_to_executable </span> - the path to executable file which will run via <span class="notranslate"> proxyexec </span> .

Example of a simple command executed via <span class="notranslate"> proxyexec </span> :
<span class="notranslate"> </span>
```
SENDMAIL=/usr/sbin/sendmail
```


Example of <span class="notranslate"> crontab </span> command execution with custom wrapper under <span class="notranslate"> root </span> (privilege escalation). The command uses two <span class="notranslate"> ALIAS </span> , that is why in the second line <span class="notranslate"> “noproceed” </span> is specified instead of wrapper name.
<span class="notranslate"> </span>
```
CRONTAB_LIST:cagefs.proxy.crontab=root:/usr/bin/crontabCRONTAB_SAVE:noproceed=root:/usr/bin/crontab
```


Sometimes hosters may have users with non unique <span class="notranslate"> UIDs </span> . Thus, <span class="notranslate"> proxyexec </span> may traverse users directory to find a specific one. That behavior turns into inappropriate if users directory is not cached locally (for example LDAP is in use).

To turn this feature off:
<span class="notranslate"> </span>
```
touch /etc/cagefs/proxy.disable.duid
```


Or to activate it back:
<span class="notranslate"> </span>
```
rm /etc/cagefs/proxy.disable.duid
```



### Custom /etc files per customer


**[4.0-5 and later]**

To create custom file in /etc directory for end user, create a directory:
<span class="notranslate"> </span>
`/etc/cagefs/custom.etc/[username]`


Put all custom files, and sub-directories into that direcotry.

For example, if you want to create custom /etc/hosts file for <span class="notranslate"> USER1 </span> , create a directory:
<span class="notranslate"> </span>
`/etc/cagefs/custom.etc/USER1`

 Inside that directory, create a file hosts, with the content for that user.

After that execute:
<span class="notranslate"> </span>
```
$ cagefsctl --update-etc USER1
```


If you are making changes for multiple users, you can run:

<span class="notranslate"> </span>
```
$ cagefsctl --update-etc
```


To remove custom file, remove it from <span class="notranslate"> /etc/cagefs/custom.etc/[USER] </span> directory, and re-run:
<span class="notranslate"> </span>
```
$ cagefsctl --update-etc
```



### Moving cagefs-skeleton directory


Sometimes you might need to move <span class="notranslate"> cagefs-skeleton </span> from <span class="notranslate"> _/usr/share_ </span> to another partition.

There are two ways:

1. If <span class="notranslate"> _/usr/share/cagefs-skeleton_ </span> is not created yet ( <span class="notranslate"> cagefsctl --init </span> wasn't executed), then execute:

<span class="notranslate"> </span>
```
$ mkdir /home/cagefs-skeleton $ ln -s /home/cagefs-skeleton /usr/share/cagefs-skeleton $ cagefsctl --init
```

 2. If <span class="notranslate"> _/usr/share/cagefs-skeleton_ </span> already exists:

<span class="notranslate"> </span>
```
$ cagefsctl --disable-cagefs $ cagefsctl --unmount-all# To ensure that the following command prints empty output: $ cat /proc/mounts | grep cagefs # if you see any cagefs entries, execute "cagefsctl --unmount-all" again.$ mv /usr/share/cagefs-skeleton /home/cagefs-skeleton $ ln -s /home/cagefs-skeleton /usr/share/cagefs-skeletoncagefsctl --enable-cagefs
```


On cPanel servers, if you place skeleton into <span class="notranslate"> _/home_   </span> directory, then you should configure the following option:

In _cPanel WHM_ choose <span class="notranslate"> _Server Configuration_ </span> and go to <span class="notranslate"> _Basic cPanel/WHM Setup_ </span> , then in <span class="notranslate"> _Basic Config_ </span> change <span class="notranslate"> _Additional home directories_   </span> default _ _ value to (not <span class="notranslate"> "home" </span> ).







### Moving /var/cagefs directory


To move /var/cagefs to another location:
<span class="notranslate"> </span>
```
$ cagefsctl --disable-cagefs$ cagefsctl --unmount-all
```


Verify that directory does not exist (if it exists - change name "cagefs.bak" to something else)
<span class="notranslate"> </span>
```
$ cp -rp /var/cagefs /new/path/cagefs$ mv /var/cagefs /var/cagefs.bak$ ln -s /new/path/cagefs /var/cagefs$ cagefsctl --enable-cagefs$ cagefsctl --remount-all
```


Verify that the following command gives empty output:
<span class="notranslate"> </span>
```
$ cat /proc/mounts | grep cagefs.bak
```


Then you can safely remove /var/cagefs.bak:
<span class="notranslate"> </span>
```
$ rm -rf /var/cagefs.bak
```


### TMP Directories


CageFS makes sure that each user has his own <span class="notranslate"> _/tmp_ </span> directory, and that directory is the part of end-user's quota.

The actual location of the directory is <span class="notranslate"> _$USER_HOME/.cagefs/tmp_ </span>

Once a day, using cron job, <span class="notranslate"> CageFS </span> will clean up user's <span class="notranslate"> _/tmp_ </span> directory from all the files that haven't been accessed during 30 days.

This can be changed by running:
<span class="notranslate"> </span>
```
$ cagefsctl --set-tmpwatch='/usr/sbin/tmpwatch -umclq 720'
```

Where 720 is the number of hours that the file had to be inaccessible to be removed.

By default this is done at 03:37 AM, but you can also force the clean up outdated files that match 'chosen period' of all user's <span class="notranslate"> _/tmp_ </span> directories without waiting for a job to be launched by <span class="notranslate"> cronjob </span> . Just run:
<span class="notranslate"> </span>
```
$ cagefsctl --tmpwatch
```

The following path will be cleaned as well:

<span class="notranslate"> _/var/cache/php-eaccelerator_   </span> (actual location _ _ <span class="notranslate"> $USER_HOME/.cagefs/var/cache/php-eaccelerator </span> )

You can configure ` ` <span class="notranslate"> tmpwatch </span> to clean custom directories inside <span class="notranslate"> CageFS </span> .

Create <span class="notranslate"> _/etc/cagefs/cagefs.ini_ </span> configuration file and specify <span class="notranslate">   _tmpwatch_dirs_ </span> directive as follows:

<span class="notranslate"> _tmpwatch_dirs=/dir1,/dir2_ </span>

After that directories <span class="notranslate"> _/dir1_ </span> and _ _ <span class="notranslate"> /dir2 </span> _ _ inside CageFS  will be cleaned automatically.

Note that actual location of those directories in real file system is <span class="notranslate"> _$USER_HOME/.cagefs/dir1_ </span> and <span class="notranslate"> _$USER_HOME/.cagefs/dir2_ </span> .

**Cleanup PHP sessions**

For cPanel servers, CageFS version 6.0-42 or higher performs cleaning of PHP sessions based on <span class="notranslate"> _session.gc_maxlifetime_ </span> and <span class="notranslate"> _session.save_path_ </span> directives specified in proper <span class="notranslate"> _php.ini_ </span> files.

<span class="notranslate"> _session.gc_maxlifetime_ </span> directive default value is 1440 seconds. Those session files will be deleted, that were created or had metadata (ctime) changes more time ago than it is specified in <span class="notranslate"> _session.gc_maxlifetime_ .  </span>

For <span class="notranslate"> Alt-PHP </span> versions <span class="notranslate"> _session.save_path_ </span> value is normally <span class="notranslate"> _/tmp_ </span> .





This applies to the following <span class="notranslate"> Alt-PHP </span> versions (or later):
<span class="notranslate"> </span>
alt-php44-4.4.9-71;
alt-php51-5.1.6-81;
alt-php52-5.2.17-107;
alt-php53-5.3.29-59;
alt-php54-5.4.45-42;
alt-php55-5.5.38-24;
alt-php56-5.6.31-7;
alt-php70-7.0.23-5;
alt-php71-7.1.9-5;
alt-php72-7.2.0-0.rc.2.2.

When using EasyApache 3, <span class="notranslate"> _session.save_path_ </span> value is normally _ _ <span class="notranslate"> /var/cpanel/php/sessions/ea3 </span> or <span class="notranslate"> _/tmp_ </span> . Seettings for EasyApache 3 are usualy taken from the file <span class="notranslate"> _/usr/local/lib/php.ini_ . </span>

When using EasyApache 4, <span class="notranslate"> _session.save_path_ </span> value is normally <span class="notranslate"> _/var/cpanel/php/sessions/ea-phpXX_ ,  </span> where <span class="notranslate"> _XX_ </span> corresponds to PHP version.

Cleaning is started by cron <span class="notranslate"> _/etc/cron.d/cpanel_php_sessions_cron_ </span> , which starts the script <span class="notranslate"> _/usr/share/cagefs/clean_user_php_sessions_ </span> twice within one hour.

The settings for ea-php are located in <span class="notranslate"> _/opt/cpanel/ea-phpXX/root/etc/php.d/local.ini_ </span> or in <span class="notranslate"> _/opt/cpanel/ea-phpXX/root/etc/php.ini_ </span> , where <span class="notranslate"> _XX_ </span> corresponds to the PHP version.

The settings for alt-php are located in _ _ <span class="notranslate"> /opt/alt/phpXX/etc/php.ini </span> files, where <span class="notranslate"> _XX_ </span> corresponds to PHP version.

The cleaning script cleans php sessions for all PHP versions ( <span class="notranslate"> _ea-php_ </span> and <span class="notranslate"> _alt-php_ </span> ) regardless of whether a version is used or selected via <span class="notranslate"> MultiPHP Manager </span> or <span class="notranslate"> PHP Selector </span> . When different <span class="notranslate"> _session.gc_maxlifetime_ </span> values are specified for the same <span class="notranslate"> _session.save_path_ </span> (for different php versions), the cleaning script will use the least value for cleaning <span class="notranslate"> _session.save_path_ </span> . So, it is recommended to specify different <span class="notranslate"> _session.save_path_ </span> for each PHP version.

Users can define custom value of _ _ <span class="notranslate"> session.gc_maxlifetime </span> via PHP Selector in order to configure PHP's garbage collector, but that will not affect the script for cleaning PHP sessions. The script cleans PHP sessions based on global values of <span class="notranslate"> _session.gc_maxlifetime_ </span> and <span class="notranslate"> _session.save_path_ </span> directives taken from files mentioned above. Settings in custom users’ <span class="notranslate"> php.ini </span> files are ignored.

**Cleanup PHP session files in Plesk**

For Plesk servers, <span class="notranslate"> CageFS </span> version 6.0-52 or higher is provided with a special cron job for removing obsolete PHP session files. Cleanup script runs once an hour (similar to how it is done in Plesk).

Each time the script runs, it performs the cleanup of the paths:

1. set by <span class="notranslate"> session.save_path </span> directive in <span class="notranslate"> /opt/alt/phpXX/etc/php.ini </span> files. If <span class="notranslate"> session.save_path </span> is missing, then <span class="notranslate"> /tmp </span> is used. Session files lifetime is set by <span class="notranslate"> session.gc_maxlifetime </span> directive. If it is not found, then 1440 seconds value is used (24 minutes, as in Plesk). Lifetime set in the file is only taken into consideration if it is longer than 1440 seconds, otherwise 1440 seconds is used. All the installed <span class="notranslate"> Alt-PHP </span> versions are processed.

2. <span class="notranslate"> /var/lib/php/session </span> . Files lifetime is only defined by Plesk script <span class="notranslate"> /usr/lib64/plesk-9.0/maxlifetime </span> . If the script is missing or returns errors, then this directory is not processed.

The following features are applied during the cleanup:

all the users with <span class="notranslate"> UID </span> higher than specified in <span class="notranslate"> /etc/login.defs </span> are processed. Each user is processed independently from one another.

only directories inside <span class="notranslate"> CageFS </span> are being cleaned. The paths of the same name in the physical  file system are not processed.

in all the detected directories, all the files with the names that correspond to <span class="notranslate"> sess_* search </span> mask are removed, the rest of the files are ignored.

the files older than specified lifetime are removed.

all non-fatal errors (lack of rights, missing directory) are ignored and do not affect the further work of the script.

**Disable PHP sessions cleanup on cPanel and Plesk**

Here is a possible workaround for PHP session expiration problem (session lives longer than it is possible in a control panel). To use your own custom PHP sessions cleanup scripts - you can turn off built-in sessions cleanup implementation in the following way: 
add `clean_user_php_sessions=false` line to `/etc/sysconfig/cloudlinux` .



### Syslog


By default, <span class="notranslate"> `/dev/log` </span> should be available inside end user's <span class="notranslate"> CageFS </span> . This is needed so that user's cronjobs and other things that user <span class="notranslate"> `dev/log` </span> would get recorded in the system log files.

This is controlled using file <span class="notranslate"> `/etc/rsyslog.d/schroot.conf` </span> with the following content:
<span class="notranslate"> </span>
```
$AddUnixListenSocket /usr/share/cagefs-skeleton/dev/log
```

To remove presence of <span class="notranslate"> `dev/log` </span> inside CageFS, remove that file, and restart rsyslog service.


### Excluding mount points




By default, all mounts from the real file system is inherited by namespaces of all <span class="notranslate"> LVEs </span> . So, destroying all <span class="notranslate"> LVEs </span> may be required in order to unmount some mount in real file system completely. Otherwise, mount point remains busy after unmounting it in the real file system because this mount exists in namespaces of <span class="notranslate"> LVEs </span> .

<span class="notranslate"> `lvectl start` </span> command saves all mounts from real file system as “default namespace” for later use in all <span class="notranslate"> LVEs </span> . <span class="notranslate"> **lve_namespaces** </span> service executes <span class="notranslate"> </span> command during startup.

In <span class="notranslate"> **lve-utils-2.0-26** </span> (and later) there is an ability to exclude specific mounts from namespaces for all <span class="notranslate"> LVEs </span> .
In order to do so, please create a file <span class="notranslate"> _/etc/container/exclude_mounts.conf_ </span> with list of mounts to exclude (one mount per line) as regular expressions, and then execute <span class="notranslate"> </span> :
<span class="notranslate"> </span>
```
# cat /etc/container/exclude_mounts.conf    ^/dir1/^/dir2$# lvectl start
```

After that, all new created <span class="notranslate"> LVEs </span> will be without <span class="notranslate"> _/dir2_ </span> mount and without mounts that start with <span class="notranslate"> _/dir1/_ </span> (like <span class="notranslate"> _/dir1/x_ ,  _/dir1/x/y_ </span> , etc). To apply changes to existing <span class="notranslate"> LVEs </span> you should recreate <span class="notranslate"> LVEs </span> :
<span class="notranslate"> </span>
```
# lvectl destroy all   # lvectl apply all
```






## Control Panel Integration


CageFS comes with a plugin for various control panels.

The plugin allows to:

·        Initialize CageFS;

·        Select [mode of operation](/cagefs/#managing-users) ;

·        See and modify the list of enabled/disabled users;

·        Update CageFS skeleton.


### cPanel


CageFS plugin for cPanel is located in <span class="notranslate"> Plugins </span> section of WHM.

It allows to initialize CageFS, select users CageFS will be enabled for, as well as update CageFS skeleton.

![](/images/_img1_zoom73.png)

To enable CageFS for a proper user (users), in <span class="notranslate"> CageFS User Manager </span> choose a user from the list on the right ( <span class="notranslate"> Disabled </span> users) and click <span class="notranslate"> Toggle </span> . The user will move to the list on the left ( <span class="notranslate"> Enabled </span> users).

To disable a user (users), choose a user from the list on the left ( <span class="notranslate"> Enabled </span> users) and click <span class="notranslate"> Disable CageFS </span> . The user will move to the list on the right ( <span class="notranslate"> Disabled </span> users).

To update CageFS Skeleton click <span class="notranslate"> Update CageFS Skeleton </span> .

![](/images/_img2_zoom71.png)




### Plesk


CageFS comes with a plugin for Plesk 10.x. It allows initializing and updating CageFS template, as well as managing users and mode of operation for CageFS.

In modules section choose CageFS:

![](/images/plesk_cagefs_icon.png)
To enable CageFS for a proper user (users), in <span class="notranslate"> CageFS User Manager </span> choose a user from the list on the right ( <span class="notranslate"> Disabled </span> users) and click <span class="notranslate"> Toggle </span> . The user will move to the list on the left ( <span class="notranslate"> Enabled </span> users).

To disable a user (users), choose a user from the list on the left ( <span class="notranslate"> Enabled </span> users) and click <span class="notranslate"> Disable CageFS </span> . The user will move to the list on the right ( <span class="notranslate"> Disabled </span> users).

To update <span class="notranslate"> CageFS Skeleton </span> click <span class="notranslate"> Update CageFS Skeleton </span> .

![](/images/plesk_cagefs_manager_disable_all.png)

### ISPManager


CageFS comes with plugin for <span class="notranslate"> ISP Manager </span> to enable/disable CageFS on per user base. In edit user section chose <span class="notranslate"> Permission </span> tab. Mark <span class="notranslate"> CageFS User Mode </span> checkbox and click <span class="notranslate"> OK </span> to apply.

![](/images/ispmanager_cagefs_user_zoom98.png)

Or you can manage global CageFS settings via CageFS menu
![](/images/_img3.jpg)
