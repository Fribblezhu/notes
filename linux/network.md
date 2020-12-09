# HOW MANAGER NETWORK SERVICE

## Different between netwok and NetworkManager
---
1. On centos 8, the unit network.service is depercated. so , when you type `serivce network restart`, it will return by 'Unit network.service not found'. but the `NetworkManager` is available, you can use `service NetworkManager restart` to replace before.
2. the network service still on centos 6 and 7.


## Errors encountered
---

### 1. NetworkManager started, but  connecting failed.
> That is occurs when i use the contos 8 on window vmware.First i try to restart the network and type `service network restart`, but it say 'unit network.service not found'.I thought maybe there was something wrong on network.service. in fact, centos 8 didn't install network service before. I was amazing and without resort.
luckly, i found it's matter on the web.
- check you __windows service__ `vmware DHCP service` started.
- check you __windows service__ `vmware NET service` started.
