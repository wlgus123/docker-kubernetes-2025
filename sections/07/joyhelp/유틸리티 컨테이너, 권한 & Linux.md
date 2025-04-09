
Linux에서 '유틸리티 컨테이너'로 작업할 때 Q&A 섹션에서 이 매우 유용한 스레드도 자세히 살펴보는 것이 좋습니다.

아래 스레드는 "유틸리티 컨테이너"로 작업할 때 Docker에서 설정한 사용자 권한과 이를 조정하는 방법에 대해 설명합니다.


## 제목: Utility Containers and Linux


This is truly an awesome course Max! Well done!

이것은 정말 멋진 과정이에요, 맥스! 잘했어요!

I wanted to point out that on a Linux system, the Utility Container idea doesn't quite work as you describe it. In Linux, by default Docker runs as the "Root" user, so when we do a lot of the things that you are advocating for with Utility Containers the files that get written to the Bind Mount have ownership and permissions of the Linux Root user. (On MacOS and Windows10, since Docker is being used from within a VM, the user mappings all happen automatically due to NFS mounts.)

리눅스 시스템에서는 유틸리티 컨테이너 아이디어가 당신이 설명한 대로 잘 작동하지 않는다는 점을 지적하고 싶었습니다. 리눅스에서는 기본적으로 도커가 "루트" 사용자로 실행되기 때문에, 유틸리티 컨테이너와 함께 당신이 주장하는 많은 작업을 수행할 때 바인드 마운트에 기록되는 파일은 리눅스 루트 사용자의 소유권과 권한을 갖게 됩니다. (MacOS와 Windows10에서는 도커가 VM 내에서 사용되기 때문에 사용자 매핑이 NFS 마운트로 인해 자동으로 발생합니다.)

So, for example on Linux, if I do the following (as you described in the course):

예를 들어 리눅스에서 제가 다음과 같은 작업을 수행하면 (당신이 과정에서 설명한 대로):


```
FROM node:14-slim

WORKDIR /app
```


$ docker build -t node-util:perm .

$ docker run -it --rm -v $(pwd):/app node-util:perm npm init

...

$ ls -la

total 16

drwxr-xr-x 3 scott scott 4096 Oct 31 16:16 ./

drwxr-xr-x 12 scott scott 4096 Oct 31 16:14 ../

drwxr-xr-x 7 scott scott 4096 Oct 31 16:14 .git/

-rw-r--r-- 1 root root 202 Oct 31 16:16 package.json


You'll see that the ownership and permissions for the package.json file are "root". But, regardless of the file that is being written to the Bind Mounted volume from commands emanating from within the docker container, e.g. "npm install", all come out with "Root" ownership.

package.json 파일의 소유권과 권한이 "root"임을 알 수 있습니다. 그러나 도커 컨테이너 내에서 발생하는 명령어, 예를 들어 "npm install"로부터 바인드 마운트된 볼륨에 기록되는 파일과 관계없이, 모든 파일은 "Root" 소유권을 갖습니다.-------


### Solution 1: Use predefined "node" user (if you're lucky)

해결책 1: 미리 정의된 "node" 사용자 사용 (운이 좋다면)


There is a lot of discussion out there in the docker community (devops) about security around running Docker as a non-privileged user (which might be a good topic for you to cover as a video lecture - or maybe you have; I haven't completed the course yet). The Official Node.js Docker Container provides such a user that they call "node".

Docker를 권한이 없는 사용자로 실행하는 것에 대한 보안에 대해 Docker 커뮤니티(DevOps)에서 많은 논의가 있습니다(이 주제는 비디오 강의로 다루기 좋은 주제일 수 있습니다 - 아니면 이미 다루었을 수도 있습니다; 저는 아직 과정을 완료하지 않았습니다). 공식 Node.js Docker 컨테이너는 "node"라고 부르는 사용자를 제공합니다.

https://github.com/nodejs/docker-node/blob/master/Dockerfile-slim.template

```
FROM debian:name-slim

RUN groupadd --gid 1000 node \

&& useradd --uid 1000 --gid node --shell /bin/bash --create-home node
```

Luckily enough for me on my local Linux system, my "scott" uid:gid is also 1000:1000 so, this happens to map nicely to the "node" user defined within the Official Node Docker Image.

다행히도 내 로컬 리눅스 시스템에서 내 "scott" uid:gid는 1000:1000이므로, 이는 공식 Node Docker 이미지 내에서 정의된 "node" 사용자와 잘 매핑됩니다.

So, in my case of using the Official Node Docker Container, all I need to do is make sure I specify that I want the container to run as a non-Root user that they make available. To do that, I just add:

그래서, 공식 Node Docker 컨테이너를 사용하는 제 경우에는, 제가 해야 할 일은 제공된 비루트 사용자로 컨테이너가 실행되도록 지정하는 것입니다. 그렇게 하려면, 저는 다음을 추가하기만 하면 됩니다:


```
FROM node:14-slim
USER node
WORKDIR /app
```


If I rebuild my Utility Container in the normal way and re-run "npm init", the ownership of the package.json file is written as if "scott" wrote the file.

내가 유틸리티 컨테이너를 일반적인 방법으로 재구성하고 "npm init"을 다시 실행하면, package.json 파일의 소유자는 "scott"이 파일을 작성한 것처럼 기록된다.

$ ls -la

total 12

drwxr-xr-x 2 scott scott 4096 Oct 31 16:23 ./

drwxr-xr-x 13 scott scott 4096 Oct 31 16:23 ../

-rw-r--r-- 1 scott scott 204 Oct 31 16:23 package.json

------------

### Solution 2: Remove the predefined "node" user and add yourself as the user

해결책 2: 미리 정의된 "node" 사용자를 제거하고 자신을 사용자로 추가합니다.


However, if the Linux user that you are running as is not lucky to be mapped to 1000:1000, then you can modify the Utility Container Dockerfile to remove the predefined "node" user and add yourself as the user that the container will run as:

그러나, 현재 실행 중인 Linux 사용자가 1000:1000에 매핑되지 않은 경우, Utility Container Dockerfile을 수정하여 미리 정의된 "node" 사용자를 제거하고 컨테이너가 실행될 사용자로 자신을 추가할 수 있습니다:-------

```
FROM node:14-slim

RUN userdel -r node

ARG USER_ID

ARG GROUP_ID

RUN addgroup --gid $GROUP_ID user

RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user

USER user

WORKDIR /app
```

-------

And then build the Docker image using the following (which also gives you a nice use of ARG):

그리고 다음을 사용하여 Docker 이미지를 빌드합니다(여기서 ARG를 잘 활용할 수 있습니다):


$ docker build -t node-util:cliuser --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) .

And finally running it with:

$ docker run -it --rm -v $(pwd):/app node-util:cliuser npm init

$ ls -la

total 12

drwxr-xr-x 2 scott scott 4096 Oct 31 16:54 ./

drwxr-xr-x 13 scott scott 4096 Oct 31 16:23 ../

-rw-r--r-- 1 scott scott 202 Oct 31 16:54 package.json


Reference to Solution 2 above: https://vsupalov.com/docker-shared-permissions/


Keep in mind that this image will not be portable, but for the purpose of the Utility Containers like this, I don't think this is an issue at all for these "Utility Containers"

이 이미지는 휴대성이 없다는 점을 염두에 두세요. 하지만 이러한 유틸리티 컨테이너의 목적을 고려할 때, 저는 이 "유틸리티 컨테이너"에 대해 전혀 문제가 되지 않는다고 생각합니다.




## 강사 답변: Maximilian

That's amazing, thank you so much for bringing this up and sharing this! I'll definitely a note regarding that to the "Utility Containers" section.

정말 놀랍습니다. 이 문제를 제기하고 공유해 주셔서 정말 감사합니다! "유틸리티 컨테이너" 섹션에 대해 꼭 메모하겠습니다.

In general, I considered diving deeper into users, container security etc. but for the moment decided against it to focus more on the general "how to use Docker" topics. But this is definitely important and hence I'll add it.

일반적으로 사용자, 컨테이너 보안 등에 대해 더 깊이 파고들어 보려고 했지만, 현재로서는 "도커 사용 방법"과 같은 일반적인 주제에 더 집중하기로 결정했습니다. 하지만 이것은 분명히 중요하므로 추가하겠습니다.

Much appreciated!

대단히 감사합니다!

Max
