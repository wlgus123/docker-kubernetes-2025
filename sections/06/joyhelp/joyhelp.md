# Docker-compose 기본 명령어 정리

리눅스에서 docker-compose 명령어를 사용하려면 docker와는 별개로 docker-compose가 설치되어 있어야 합니다. 

```
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
echo $PATH
```

- 버전 확인
```
docker-compose --version
```

- docker-compose up
  컨테이너를 생성 및 실행합니다.
```
docker-compose up [옵션][서비스명]
```

옵션
설명

-d
백그라운드 실행

--no-deps
링크 서비스 실행하지 않음

--build
이미지 빌드

-t
타임아웃을 지정(기본 10초)
참고로 특정 서비스들의 경우 백그라운드로 실행하지 않으면 컨테이너가 생성 및 실행되며 바로 종료될 수 있습니다.

- docker-compose down
  네트워크 정보, 볼륨, 컨테이너들을 일괄 정지 및 삭제 처리 합니다.
```
docker-compose down
```

- docker-compose ps
  현재 동작중인 컨테이너들의 상태를 확인
```
docker-compose ps
```



- docker-compose logs
  컨테이너들의 로그를 출력합니다.
```
docker-compose logs
```

- docker-compose run
  docker-compose up 명령어를 이용해 생성 및 실행된 컨테이너에서 임의의 명령을 실행하기 위해 사용합니다.
  컨테이너들을 모두 삭제할 경우 docker-compose start가 아닌, docker-compose up으로 다시 컨테이너들을 생성 해주어야 합니다.
```
docker-compose run
```

만약 특정 서비스에서 /bin/bash를 실행시켜 쉘 환경으로 진입하고 싶으면 아래와 같은 명령어를 이용하면 됩니다. 참고로 서비스명과 컨테이너명은 다릅니다.
서비스명은 docker-compose.yml의 services: 밑에 작성한 서비스 이름 입니다.
```
# docker-compose run [서비스명][명령]
docker-compose run redis /bin/bash
```

- docker-compose (start/stop/pause/unpause/restart)
  여러개의 서비스 또는 특정 서비스를 시작/정지/일시정지/재시작을 할 수 있습니다.
```
# 서비스 시작
docker-compose start

# 서비스 정지
docker-compose stop

# 서비스 일시 정지
docker-compose pause

# 서비스 일시 정지 해제
docker-compose unpause

# 서비스 재시작
docker-compose restart
```
각각의 설정 뒤에 서비스명을 붙이면 특정 서비스만 제어할 수 있습니다.(ex. docker-compose restart [서비스명])

- docker-compose rm
  docker-compose로 생성한 컨테이너들을 일괄 삭제 합니다.(삭제 전, 관련 컨테이너들을 종료시켜 두어야 합니다.)
```
docker-compose rm
```

- docker-compose kill
  실행중인 컨테이너를 강제로 정지시킵니다.
  -s옵션을 사용하여 시그널을 지정해 줄 수 있는데, 아래 코드에서는 SIGINT를 사용하였습니다. -s 옵션을 사용하지 않고 docker-compose kill만 사용할 경우 SIGKILL이 전송됩니다.
  kill 뒤에 서비스를 지정하여 특정 서비스만 kill 할 수 있습니다.
```
# docker-compose kill [옵션]
docker-compose kill -s SIGINT
```

- docker-compose port
  서비스 프라이빗 포트 번호의 설정을 확인할 수 있습니다.
```
# docker-compose port [서비스명][프라이빗 포트 번호]
docker-compose port nginx 80
```

- docker-compose config
  docker-compose 구성 파일의 내용을 확인할 수 있습니다. docker-compose.yml의 내용을 출력해주므로 많이 쓸 일은 없습니다.
```
docker-compose config
```
