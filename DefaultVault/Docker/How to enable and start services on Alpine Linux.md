## View status of all services

Type the following command:  
`# rc-status`

Runlevel: default
 crond                                  [  started  ]
 networking                             [  started  ]
Dynamic Runlevel: hotplugged
Dynamic Runlevel: needed/wanted
Dynamic Runlevel: manual

The default run level is called default, and it started crond and networking service for us.

## View service list

Type the following command:  
`# rc-status --list`  
Sample outputs:

boot
nonetwork
default
sysinit
shutdown

You can change run level using the rc command:  

```
# rc {runlevel}   
# rc boot   
# rc default   
# rc shutdown
```


1. **boot** – Generally the only services you should add to the boot runlevel are those which deal with the mounting of filesystems, set the initial state of attached peripherals and logging. Hotplugged services are added to the boot runlevel by the system. All services in the boot and sysinit runlevels are automatically included in all other runlevels except for those listed here.
2. **single** – Stops all services except for those in the sysinit runlevel.
3. **reboot** – Changes to the shutdown runlevel and then reboots the host.
4. **shutdown** – Changes to the shutdown runlevel and then halts the host.
5. **default** – Used if no runlevel is specified. (This is generally the runlevel you want to add services to.)

To see manually started services, run:  
`# rc-status --manual   apache2`  
To see crashed services, run:  
`# rc-status --crashed`

## How to list all available services

Type the following command:  
`# rc-service --list   # rc-service --list | grep -i nginx`

If apache2/nginx not installed, try the [apk command](https://www.cyberciti.biz/faq/10-alpine-linux-apk-command-examples/ "10 Alpine Linux apk Command Examples") to install it:  
`# apk add apache2`

## How to add/enable service at boot time

The syntax is:  
`rc-update add {service-name} {run-level-name}`  
To add apache2 service at boot time, run:  
`# rc-update add apache2`  
OR  
`# rc-update add apache2 default`  
Sample outputs:

 * service apache2 added to runlevel default

### Removing service at boot time on Alpine Linux

Pass the del as follows:  
`rc-update del {service-name-here}`  
For example, remove vnstatd service:  
`# rc-update del vnstatd`

 * service vnstatd removed from runlevel default

If you wish to add that service again, simple run:  
`# rc-update add vnstatd`

## How to start/stop/restart services on Alpine Linux

The syntax is as as follows:

### How to start service

The syntax is:  
`# rc-service {service-name} start`  
OR  
`# /etc/init.d/{service-name} start`

### How to stop service

The syntax is:  
`# rc-service {service-name} stop`  
OR  
`# /etc/init.d/{service-name} stop`

### How to restart service

The syntax is:  
`# rc-service {service-name} restart`  
OR  
`# /etc/init.d/{service-name} restart`  
Thus to stop, start, and restart an Apache2 service:  
`# rc-service apache2 stop   # rc-service apache2 start   ### [ edit config file ] ###   # vi /etc/apache2/httpd.conf   ### [ restart apache 2 after editing the file ] ###   # rc-service apache2 restart`  
[![Alpine Linux Init System Commands](https://www.cyberciti.biz/media/new/faq/2017/12/Alpine-Linux-Init-System-Commands.jpg)](https://www.cyberciti.biz/media/new/faq/2017/12/Alpine-Linux-Init-System-Commands.jpg)

## Summing up

You leaned service managment when usign Alpine Linux on your cloud server/VM. For more info [see](https://wiki.alpinelinux.org/wiki/Alpine_Linux_Init_System) Alpine Linux project.

