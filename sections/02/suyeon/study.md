## 파일 복사

`docker cp [복사할 파일 경로] [복사할 컨테이너 이름]:/[복사될 파일 이름]`

ex) docker cp dummy/. boring_vaughan:/test
dummy 폴더 하단의 모든 내용을 boring_vaughan라는 이름의 컨테이너 안의 test 폴더에 복사(폴더가 없다면 생성 후 복사)

**사용하는 방법**

1. 소스코드가 변경되었을 때, 컨테이너를 다시 시작하지 않아도 된다.
   => 변경된 파일을 잊어버릴 수 있음.
   => 현재 실행중인 파일은 수정할 수 없음.
2. 로그 파일에 대해 컨테이너에서 로컬 파일로 복사할 수 있음

## 이미지와 컨테이너의 이름을 지정하는 옵션

컨테이너와 태그에 고유한 이름 명명

`docker run --help` : 도커 모든 명령어 확인

`docker run -p 3000:80 -d --rm --name [고유한 이름] [Image ID]`

> 이미지에 이름 정하기 === 태그  
> 이미지 리포지토리(name) : 태그(tag)  
> DockerFile에서 FROM node:18(버전)  
> 이런 방식으로 사용됨

> node=name  
> 18=tag  
> 태그: Docker Hub나 Docker에 정보를 알려주는 태그로 사용

`docker build -t [name:tag]`

`docker run -p 3000:80 -d --rm --name [고유한 이름] [name:tag] => 이미지 ID 대신 tag 사용가능`
