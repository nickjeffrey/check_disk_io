# check_disk_io
nagios check for disk IO on UNIX-like systems

This script is executed remotely on a monitored system by the NRPE or check_by_ssh methods available in nagios.

If you are using the check_by_ssh method, you will need a section in the services.cfg file on the nagios server that looks similar to the following. This assumes that you already have ssh key pairs configured.
      define service {
              use                             generic-24x7-service
              hostgroup_name                  all_aix,all_linux,all_freebsd,all_macos
              service_description             disk IO
              check_command                   check_by_ssh!/usr/local/nagios/libexec/check_disk_io
              }

If you are using the check_nrpe method, you will need a section in the services.cfg file on the nagios server that looks similar to the following.
    define service{
           use                             generic-24x7-service
           hostgroup_name                  all_aix,all_linux,all_freebsd,all_macos
           service_description             disk IO
           check_command                   check_nrpe!check_disk_io
           }
If using NRPE, you will also need a section defining the NRPE command in the /usr/local/nagios/nrpe.cfg file that looks like this:
    command[check_disk_io]=/usr/local/nagios/libexec/check_disk_io
