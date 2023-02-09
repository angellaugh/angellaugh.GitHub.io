# docker

sudo docker run -i -t --name mps_controller_ali1 -v /mnt/qalogserver/qa:/mnt/qalogserver/qa docker.west.isilon.com/test-controller:latest /bin/bash 



```
docker tag 4be973006224 docker.west.isilon.com/test_ali1:202301
docker push docker.west.isilon.com/test_ali1:202301
```
看来如上两句是对的，只是我的client没权限，需要配置，明天再搞吧！
这两句的id是image id，不是docker container id

啊哈哈哈 搞定啦 nice
需要先logout `docker logout docker.west.isilon.com`

并且，要用root 用户

```
root@remotedev-ali1-rdu:/var/lib/docker/image/overlay2# docker tag 4be973006224 docker.west.isilon.com/test_ali1:202301
root@remotedev-ali1-rdu:/var/lib/docker/image/overlay2# docker push docker.west.isilon.com/test_ali1:202301
The push refers to repository [docker.west.isilon.com/test_ali1]
da7a14b0685e: Preparing
da8ffd2b0659: Preparing
c979db3cc13e: Preparing
c5f516bbdd98: Preparing
0dcafaecafc6: Preparing
1c6c8aeef0ca: Waiting
57389385e606: Waiting
31fb404eb679: Waiting
d7bf06c91c69: Waiting
26769948eeef: Waiting
f3802cb5d0cd: Waiting
d0eea63fec60: Waiting
1ca8d60aa482: Waiting
6d02f469e80e: Waiting
fb995dc7b7a7: Waiting
4f08aa748d17: Waiting
cbdd90d2d023: Waiting
45b82476d2c2: Waiting
682462892c66: Waiting
37b04561ca02: Waiting
7ae182f57c43: Waiting
543cf2341d82: Waiting
00fdd5d9c4dd: Waiting
3c043f429a21: Waiting
38467d670508: Waiting
77b174a6a187: Waiting
unauthorized: The client does not have permission to push to the repository.
```


啊哈哈哈 搞定啦 nice
需要先logout `docker logout docker.west.isilon.com`
```
root@remotedev-ali1-rdu:/var/lib/docker/image/overlay2# docker logout docker.west.isilon.com
Removing login credentials for docker.west.isilon.com
root@remotedev-ali1-rdu:/var/lib/docker/image/overlay2# docker push docker.west.isilon.com/test_ali1:202301
The push refers to repository [docker.west.isilon.com/test_ali1]
da7a14b0685e: Pushed
da8ffd2b0659: Layer already exists
c979db3cc13e: Layer already exists
c5f516bbdd98: Layer already exists
0dcafaecafc6: Layer already exists
1c6c8aeef0ca: Layer already exists
57389385e606: Layer already exists
31fb404eb679: Layer already exists
d7bf06c91c69: Layer already exists
26769948eeef: Layer already exists
f3802cb5d0cd: Layer already exists
d0eea63fec60: Layer already exists
1ca8d60aa482: Layer already exists
6d02f469e80e: Layer already exists
fb995dc7b7a7: Layer already exists
4f08aa748d17: Layer already exists
cbdd90d2d023: Layer already exists
45b82476d2c2: Layer already exists
682462892c66: Layer already exists
37b04561ca02: Layer already exists
7ae182f57c43: Layer already exists
543cf2341d82: Layer already exists
00fdd5d9c4dd: Layer already exists
3c043f429a21: Layer already exists
38467d670508: Layer already exists
77b174a6a187: Layer already exists
202301: digest: sha256:8cd8c10b500bd3daa0bcbdf39d1a093acacc6428df892d7bc0412f900b0c11f4 size: 5769
root@remotedev-ali1-rdu:/var/lib/docker/image/overlay2#
```
