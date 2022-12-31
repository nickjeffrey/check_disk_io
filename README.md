# check_disk_io
nagios check for disk IO on UNIX-like systems

Tested on AIX, FreeBSD, Linux, MacOS.  

Not tested on SunOS, HPUX, OpenBSD, NetBSD, but will likely work with minor updates.

# Usage
All parameters are optional, defaults to --busy=40 --latency=30 for all disk devices.
```
    check_disk_io
    check_disk_io --verbose
    check_disk_io --help
    check_disk_io --busy=##                     #warn if disk percent busy is >=  ## (defaults to 40)
    /check_disk_io --latency=##                  #warn if disk latency      is >=  ## milliseconds (defaults to 30)
    check_disk_io --exclude=hdisk7,hdisk8       #example disk exclusion for AIX    (does not support wildcards)
    check_disk_io --exclude=sda,sdb,dm-0        #example disk exclusion for Linux  (does not support wildcards)
    check_disk_io --include=device1,device2     #example disk inclusion, only report on these disks (does not support wildcards)
```

This script is executed remotely on a monitored system by the NRPE or check_by_ssh methods available in nagios.

If you are using the check_by_ssh method, you will need a section in the services.cfg file on the nagios server that looks similar to the following. This assumes that you already have ssh key pairs configured.
````
    define service {
              # optional parameters --busy=## --latency=## --include=device1,device2 --exclude=device3,device4
              use                             generic-24x7-service
              hostgroup_name                  all_aix,all_linux,all_freebsd,all_macos
              service_description             disk IO
              check_command                   check_by_ssh!/usr/local/nagios/libexec/check_disk_io
              }
````
If you are using the check_nrpe method, you will need a section in the services.cfg file on the nagios server that looks similar to the following.
````
    define service{
           use                             generic-24x7-service
           hostgroup_name                  all_aix,all_linux,all_freebsd,all_macos
           service_description             disk IO
           check_command                   check_nrpe!check_disk_io
           }
````
If using NRPE, you will also need a section defining the NRPE command in the /usr/local/nagios/nrpe.cfg file that looks like this:
````
    command[check_disk_io]=/usr/local/nagios/libexec/check_disk_io
````

Sample output on a Linux system with one disk:
<img src=images/linux.png>

Sample output on an AIX system with many disks:
<img src=images/aix.png>
