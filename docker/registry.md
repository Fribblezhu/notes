## Basic Command

#### 1.Start your registry

```
docker run -d -p 5000:5000 --name registry registry
```
#### 2.Pull (or build) some image from the hub
```
docker pull ubuntu
```
#### 3.Tag the image so that it points to your registry
```
docker image tag ubuntu localhost:5000/myfirstimage
```
#### 4.Push it
```
docker push localhost:5000/myfirstimage
```
#### 5.Pull it back
```
docker pull localhost:5000/myfirstimage
```
#### 6.Now stop your registry and remove all data
```
docker container stop registry && docker container rm -v registry
```
