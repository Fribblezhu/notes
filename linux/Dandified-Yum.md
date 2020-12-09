DNF is simply the next generation package manager(after YUM) for RPM based Linux distributions such as CentOS, RHEL, Fedora etc.
- updating package repository cache
``` shell
dnf makecache
```

- listing enabled or disabled package repositories
``` shell
dnf repolist --all
dnf repolist --enabled
dnf repolist --disabled
```

- listing package
``` shell
dnf list --all
dnf list --installed
```

- searching for packages
```shell
dnf search "<package name or summary contains>"
dnf repoquery "*<package name>*"
```

- searching for packages that provides specific file
``` shell
sudo dnf provides */<filename>
```

- learning more about packages
```shell
dnf info <package name>
```

- installing packages
```shell
dnf install <package name>
```
- reinstalling packages
``` shell
dnf reinstall <package name>
```
- removing packages
```shell
dnf remove <package name>
```
- doing a system upgrade
```shell
dnf check-update
dnf upgrade-minimal
dnf upgrade
```
- clear caches
```shell
dnf clean all
```
- remove unnecessary packages
```shell
dnf autoremove
```
