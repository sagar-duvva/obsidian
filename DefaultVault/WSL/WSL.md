
WSL (Windows Subsystem for Linux)
## Installing WSL

Install WSL
```
wsl --install
```
Install WSL along with default distribution
```
wsl --install
wsl --install -d Debian
```
### Install Linux Distribution
first list available distros
```
wsl --list --online
```

install the distro you want
```
wsl --install -d <Distro_Name>
```


### WSL Commands
start new WSL session
```
wsl ls -la
```

list all installed wsl linux distribution
```
wsl -l
wsl --list
```

lists installed distribution with detailed info
```
wsl -l -v
wsl --list --verbose
```

Delete (unregister) the unwanted distro/instance
```
wsl --unregister <DistroName> or wsl --unregister <InstanceName>
wsl --unregister Ubuntu
```

starts a WSL session with specified linux distribution
```
wsl -d Ubuntu
wsl --distribution kali-linux
```


shutdown all running WSL Instances
```
wsl --shutdown
```

Terminates a running instance
```
wsl --terminate <distribution> or wsl -t <distribution>
wsl --terminate Ubuntu
wsl -t kali-linux
```

Sets the WSL Version (1 or 2) for the specified Linux Distribution
```
wsl --set-version <distribution> <version>
wsl --set-version Ubuntu 2 #for version 2
wsl --set-version Debian 1 #for version 1
```

changes the default install version for new distros
```
wsl --set-default-version <version>
```

Sets the default Linux distribution for WSL Session
```
wsl --default <distribution>
wsl --default Ubuntu
```


Set the distribution as default
```
wsl --set-default <Distro> or wsl -s <distro>
```

Shows the status of WSL, Default version and kernel info
```
wsl --status
```

exports the specified distribution to a tarball file (backup)
```
wsl --export <distribution> <file.tar>
wsl --export Ubuntu ubuntu_backup.tar
```

Imports a new distribution from a tarball file to the given path
```
wsl --import <distribution> <install_path> <file.tar>
wsl --import MyUbuntu C:\WSL\MyUbuntu ubuntu_backup.tar
```

Mounts a physical disk to WSL 2 for access to linux
```
wsl --mount <disk_path> <mount_point>
wsl --mount \\. \PHYSICALDRIVE2
```

Unmounts a mounted disk from WSL2
```
wsl --unmount <mount_point>
wsl --unmount /mnt/w
```


```
wsl --install <distro> --name <name> #sets name of the distro
wsl --install <Distro> --enable-wsl1 #enables WSL1 support
wsl --install <Distro> --location <installation\path\distro> #set the install path for the distro

wsl --install <Distro> --version <version> #specifies the version to use for the distro

wsl --install <Distro> --vhd-size <memorystring> #specify the size of the disk

wsl --install <Distro> --no-launch or wsl --install <Distro> -n
#donot launch the distro after installation

```




```
wsl --exec <command> or wsl -e <command> #executes the command

wsl --user <username> or wsl -w <username> #run as the specified user

wsl --system #launches a shell for system distribution
```