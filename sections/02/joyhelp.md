## 32. 이미 실행 중인 컨테이너에 연결하기

디폴트로 '-d' 없이 컨테이너를 실행하면, "attached모드"로 실행됩니다.
detached 모드(예: -d)로 컨테이너를 시작한 경우에는 다음 명령을 사용하여 컨테이너를 다시 시작하지 않고도 컨테이너에 연결할 수 있습니다.

docker attach CONTAINER
이는 CONTAINER라는 ID 또는 이름으로 실행 중인 컨테이너에 연결합니다.

## 33. 인터렉티브 모드로 들어가기

## 34. 이미지 & 컨테이너 삭제하기

## 35. 중지된 컨테이너 자동 제거하기

### [Docker 명령어 요약] 

## docker prune 커맨드
- docker container prune : 중지된 모든 컨테이너를 삭제
- docker image prune : 이름 없는 모든 이미지를 삭제
- docker network prune : 사용되지 않는 도커 네트워크를 모두 삭제
- docker volume prune : 도커 컨테이너에서 사용하지 않는 모든 도커 볼륨을 삭제

## docker 컨테이너 로그 확인
```
docker logs -f --tail --it $container-ps-id

docker logs --follow --tail --it $container-ps-id
```
