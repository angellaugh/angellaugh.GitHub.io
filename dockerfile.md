Dockerfile

1. FROM        ---> 必须要有，当前镜像基于哪个镜像
2. WORKDIR     ---> will be created if it does not exit
3. COPY        ---> copy new files and directories to the image's filesystem
4. ADD         ---> like COPY, but ADD can refer URL.
5. RUN         ---> execute commands inside a shell, 构建时候运行的
6. CMD         ---> set the default executable and parameters for this executing container  指定镜像容器起来之后，运行的脚本，运行后，container生命周期就结束了。
                    CMD  ["cat", "1.txt"]

7. ENTRYPOINT  ---> 和 CMD 一样， 都是容器起来之后，运行的核心脚本，
> **如果ENTRYPOINT 非 json，则以ENTRYPOINT为准，CMD 无效；**
> **如果ENTRYPOINT 和 CMD 都是json，则以ENTRYPOINT + CMD 拼接成的shell 为准。**



```
FROM alpine
WORKDIR /app
COPY  testdockerfile/  /app
RUN echo aaaaaa >> 1.txt
ARG B=10
ENV A $B
CMD cat 1.txt
ENTRYPOINT echo $A
~/PycharmProjects/05work 🔥» docker run testarg                               
10
(venv) 🔥---
```


```dockerfile
FROM alpine
WORKDIR /app
COPY  src/  /app
RUN echo 321 >> 1.txt
CMD tail -f 1.txt   # 阻塞住的 换成cat
ENV A=10      # A 10
ENTRYPOINT echo $A
```


```
~/PycharmProjects/05work 🔥» docker build -t testdockerfilecontainer .  
~/PycharmProjects/05work 🔥» docker run testdockerfilecontainer                
321
~/Downloads 🔥» docker ps                                                     
CONTAINER ID   IMAGE                     COMMAND                  CREATED          STATUS          PORTS     NAMES
781f48a818d3   testdockerfilecontainer   "/bin/sh -c 'tail -f…"   18 seconds ago   Up 17 seconds             blissful_hawking
```


```
FROM alpine
WORKDIR /app
COPY  src/  /app
RUN echo aaaaaaa >> 1.txt
CMD cat 1.txgt

~/PycharmProjects/05work 🔥» docker build -t testdockerfilecontainer .
~/PycharmProjects/05work 🔥» docker run testdockerfilecontainer
aaaaaaa

~/Downloads 🔥» docker ps                                                     
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS     NAMES
781f48a818d3   167666e5e6fc   "/bin/sh -c 'tail -f…"   7 minutes ago   Up 7 minutes             blissful_hawking
```
依然阻塞，因为tail 始终在监听


```dockerfile
FROM docker.west.isilon.com/eng-pact/jenkins-ci-edge/jenkins-python:1.9.2

COPY . /workspace
```


`docker inspect nginx:1.13`

8. EXPOSE         ---> 映射port 
9. VOLUME /a/b    ---> 将docker中的卷映射到 宿主机
类似于docker run -p -v

10. ENV            ---> 指定系统环境变量，构建和运行时都有效  也可以在docker run -e 指定环境变量
11. ARG            ---> 只在构建时生效, ARG可以通过传递给ENV，然后在CMD or ENTRYPOINT中引用
                        ARG必须写成等于号才能生效，ENV 可以写成空格。

`docker build -t containername --build-arg B=1333 .`

```
~/PycharmProjects/05work 🔥» docker run testarg                               
1333

```
12. LABEL            ---> 元数据信息，一般写入第二行， key=value, 无实质作用
13. ONBUILD          ---> 如果其他的镜像，使用时才生效

如下文，第一次run，
1. docker build -t testonbuild .
2. docker run testonbuild
3. 输出： 10

另外写个dockerfile 内容`FROM testonbuild`
1. docker build -t teston
2. docker run teston
3. 输出：1000
4. docker run -e A=999999
5. 输出： 9999999

```
FROM alpine
WORKDIR /app
COPY  testdockerfile/  /app
RUN echo aaaaaa >> 1.txt
ARG B=10
ENV A $B
ONBUILD ENV A=1000
CMD cat 1.txt
ENTRYPOINT echo $A
```

14. STOPSIGNAL   ---> 用什么信号源去停止container
15. HEALTHCHECK
16. SHELL        ---> 基于哪种shell /bin/sh, /bin/bash
17. USER         ---> 不指定的话，是root
USER  用户名:用户组   或
USER  用户id:组id


新版本的docker还支持多个from分阶段构建，这样可以把一些编译之类的工作放在第一个from的镜像里，直接把编译结果copy到第二个from的镜像中，从而减小最终的镜像大小。
```
这个其实是COPY --from的一个用法：
COPY --from=xxx /src /des
这里的from指定文件来源，默认不写是宿主机文件系统，xxx可以是另一个镜像，比如nginx，当然也可以是你说的多from啦，另一个from段构建成的一个临时镜像，需要给临时镜像一个临时名字，使用AS用法。形如：
FROM alpine AS haha
......
.......
FROM ubuntu 
.....
COPY --from=haha /src /des

