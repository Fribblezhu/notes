### Official Guide Page
https://docs.docker.com/config/daemon/


### Configure the Docker daemon

There are two ways to configure the Docker daemon:

1.    Use a JSON configuration file. This is the preferred option, since it keeps all configurations in a single place.
2.    Use flags when starting `dockerd`.

You can use both of these options together as long as you don’t specify the same option both as a flag and in the JSON file. If that happens, the Docker daemon won’t start and prints an error message.

To configure the Docker daemon using a JSON file, create a file at `/etc/docker/daemon.json` on Linux systems, or `C:\ProgramData\docker\config\daemon.json` on Windows. On MacOS go to the whale in the `taskbar > Preferences > Daemon > Advanced`.

Here’s what the configuration file looks like:
```
{
  "debug": true,
  "tls": true,
  "tlscert": "/var/docker/server.pem",
  "tlskey": "/var/docker/serverkey.pem",
  "hosts": ["tcp://192.168.59.3:2376"]
}
```
With this configuration the Docker daemon runs in debug mode, uses TLS, and listens for traffic routed to 192.168.59.3 on port 2376. You can learn what configuration options are available in the dockerd reference docs

You can also start the Docker daemon manually and configure it using flags. This can be useful for troubleshooting problems.

Here’s an example of how to manually start the Docker daemon, using the same configurations as above:
```
dockerd --debug \
  --tls=true \
  --tlscert=/var/docker/server.pem \
  --tlskey=/var/docker/serverkey.pem \
  --host tcp://192.168.59.3:2376
```
You can learn what configuration options are available in the [dockerd reference docs](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-socket-option), or by running:
```
dockerd --help
```

This is a full example of the allowed configuration options on Linux:
```
{
  "authorization-plugins": [],
  "data-root": "",
  "dns": [],
  "dns-opts": [],
  "dns-search": [],
  "exec-opts": [],
  "exec-root": "",
  "experimental": false,
  "features": {},
  "storage-driver": "",
  "storage-opts": [],
  "labels": [],
  "live-restore": true,
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file":"5",
    "labels": "somelabel",
    "env": "os,customer"
  },
  "mtu": 0,
  "pidfile": "",
  "cluster-store": "",
  "cluster-store-opts": {},
  "cluster-advertise": "",
  "max-concurrent-downloads": 3,
  "max-concurrent-uploads": 5,
  "default-shm-size": "64M",
  "shutdown-timeout": 15,
  "debug": true,
  "hosts": [],
  "log-level": "",
  "tls": true,
  "tlsverify": true,
  "tlscacert": "",
  "tlscert": "",
  "tlskey": "",
  "swarm-default-advertise-addr": "",
  "api-cors-header": "",
  "selinux-enabled": false,
  "userns-remap": "",
  "group": "",
  "cgroup-parent": "",
  "default-ulimits": {
    "nofile": {
      "Name": "nofile",
      "Hard": 64000,
      "Soft": 64000
    }
  },
  "init": false,
  "init-path": "/usr/libexec/docker-init",
  "ipv6": false,
  "iptables": false,
  "ip-forward": false,
  "ip-masq": false,
  "userland-proxy": false,
  "userland-proxy-path": "/usr/libexec/docker-proxy",
  "ip": "0.0.0.0",
  "bridge": "",
  "bip": "",
  "fixed-cidr": "",
  "fixed-cidr-v6": "",
  "default-gateway": "",
  "default-gateway-v6": "",
  "icc": false,
  "raw-logs": false,
  "allow-nondistributable-artifacts": [],
  "registry-mirrors": [],
  "seccomp-profile": "",
  "insecure-registries": [],
  "no-new-privileges": false,
  "default-runtime": "runc",
  "oom-score-adjust": -500,
  "node-generic-resources": ["NVIDIA-GPU=UUID1", "NVIDIA-GPU=UUID2"],
  "runtimes": {
    "cc-runtime": {
      "path": "/usr/bin/cc-runtime"
    },
    "custom": {
      "path": "/usr/local/bin/my-runc-replacement",
      "runtimeArgs": [
        "--debug"
      ]
    }
  },
  "default-address-pools":[
    {"base":"172.80.0.0/16","size":24},
    {"base":"172.90.0.0/16","size":24}
  ]
}
```
>    Note:
>    You cannot set options in daemon.json that have already been set on daemon startup as a flag. On systems that use systemd to start the Docker daemon, -H is already set, so you cannot use the hosts key in daemon.json to add listening addresses. See https://docs.docker.com/engine/admin/systemd/#custom-docker-daemon-options for how to accomplish this task with a systemd drop-in file.


### Docker daemon directory

The Docker daemon persists all data in a single directory. This tracks everything related to Docker, including containers, images, volumes, service definition, and secrets.

By default this directory is:

1. /var/lib/docker on Linux.
2. C:\ProgramData\docker on Windows.

You can configure the Docker daemon to use a different directory, using the data-root configuration option.

Since the state of a Docker daemon is kept on this directory, make sure you use a dedicated directory for each daemon. If two daemons share the same directory, for example, an NFS share, you are going to experience errors that are difficult to troubleshoot.

### View stack traces

The Docker daemon log can be viewed by using one of the following methods:

1.    By running journalctl -u docker.service on Linux systems using systemctl
2.    /var/log/messages, /var/log/daemon.log, or /var/log/docker.log on older Linux systems

>    Note: It is not possible to manually generate a stack trace on Docker Desktop for Mac or Docker Desktop for Windows. However, you can click the Docker taskbar icon and choose Diagnose and feedback to send information to Docker if you run into issues.

Look in the Docker logs for a message like the following:
```
...goroutine stacks written to /var/run/docker/goroutine-stacks-2017-06-02T193336z.log
...daemon datastructure dump written to /var/run/docker/daemon-data-2017-06-02T193336z.log
```
The locations where Docker saves these stack traces and dumps depends on your operating system and configuration. You can sometimes get useful diagnostic information straight from the stack traces and dumps. Otherwise, you can provide this information to Docker for help diagnosing the problem.
