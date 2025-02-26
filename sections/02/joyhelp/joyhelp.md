## 32. 이미 실행 중인 컨테이너에 연결하기

디폴트로 '-d' 없이 컨테이너를 실행하면, "attached모드"로 실행됩니다.

detached 모드(예: -d)로 컨테이너를 시작한 경우에는 다음 명령을 사용하여 컨테이너를 다시 시작하지 않고도 컨테이너에 연결할 수 있습니다.

docker attach CONTAINER

이는 CONTAINER라는 ID 또는 이름으로 실행 중인 컨테이너에 연결합니다.

## 33. 인터렉티브 모드로 들어가기

`docker exec -it` 명령어는 실행 중인 Docker 컨테이너 내부에서 명령어를 실행할 때 사용됩니다. 
이 명령어는 주로 컨테이너 내부로 진입하여 쉘을 실행하거나 특정 작업을 수행하는 데 유용합니다.

`docker exec` 명령어는 다음과 같이 구성됩니다:

```sh
docker exec -it <container_name_or_id> <command>
```

- `-i`: 인터랙티브 모드(interactive mode)를 의미합니다. 이를 통해 표준 입력(stdin)을 컨테이너에 연결합니다.
- `-t`: TTY(가상 터미널)를 할당합니다. 이를 통해 컨테이너 내에서 터미널 기능을 사용할 수 있게 됩니다.
- `<container_name_or_id>`: 명령어를 실행할 대상 컨테이너의 이름 또는 ID를 지정합니다.
- `<command>`: 컨테이너 내에서 실행할 명령어를 지정합니다.

### 예시

컨테이너 내에서 bash 셸을 실행하는 예시:

```sh
docker exec -it my_container /bin/bash
```

위 명령어는 `my_container`라는 이름의 컨테이너 내부로 진입하여 `/bin/bash` 셸을 실행합니다. 

이제 터미널에서 컨테이너 내부의 bash 셸을 사용할 수 있습니다.

컨테이너 내에서 특정 명령어를 실행하는 예시:

```sh
docker exec -it my_container ls /app
```

위 명령어는 `my_container`라는 이름의 컨테이너 내에서 `/app` 디렉터리의 내용을 나열하는 `ls` 명령어를 실행합니다.

## 34. 이미지 & 컨테이너 삭제하기

Docker 이미지와 컨테이너를 삭제하는 방법은 다음과 같습니다:

### Docker 컨테이너 삭제하기

1. **실행 중인 컨테이너 정지**:
   먼저, 삭제하려는 컨테이너가 실행 중이라면 이를 정지해야 합니다. 실행 중인 컨테이너를 정지하려면 다음 명령어를 사용하세요:

   ```sh
   docker stop <container_id_or_name>
   ```

2. **정지된 컨테이너 삭제**:
   정지된 컨테이너를 삭제하려면 다음 명령어를 사용하세요:

   ```sh
   docker rm <container_id_or_name>
   ```

3. **모든 정지된 컨테이너 삭제**:
   모든 정지된 컨테이너를 한 번에 삭제하려면 다음 명령어를 사용할 수 있습니다:

   ```sh
   docker container prune
   ```

### Docker 이미지 삭제하기

1. **특정 이미지 삭제**:
   특정 이미지를 삭제하려면 다음 명령어를 사용하세요:

   ```sh
   docker rmi <image_id_or_name>
   ```

2. **사용하지 않는 모든 이미지 삭제**:
   사용되지 않는 모든 이미지를 한 번에 삭제하려면 다음 명령어를 사용할 수 있습니다:

   ```sh
   docker image prune
   ```

3. **중간 이미지 레이어 삭제**:
   중간 이미지 레이어(중간 생성된 이미지를 포함한 이미지 레이어)를 삭제하려면 다음 명령어를 사용할 수 있습니다:

   ```sh
   docker image prune -a
   ```

### 예시

1. **컨테이너 삭제 예시**:

   ```sh
   # 컨테이너 정지
   docker stop my_container

   # 컨테이너 삭제
   docker rm my_container

   # 모든 정지된 컨테이너 삭제
   docker container prune
   ```

2. **이미지 삭제 예시**:

   ```sh
   # 이미지 삭제
   docker rmi my_image

   # 사용하지 않는 모든 이미지 삭제
   docker image prune

   # 중간 이미지 레이어 삭제
   docker image prune -a
   ```


## 35. 중지된 컨테이너 자동 제거하기

Docker에서는 컨테이너가 종료될 때 자동으로 제거되도록 설정할 수 있는 옵션을 제공합니다. 이를 위해 `docker run` 명령어에 `--rm` 옵션을 사용하면 됩니다.

### 자동 제거 옵션 예시

컨테이너가 종료되면 자동으로 제거되도록 실행하려면 다음과 같이 `--rm` 옵션을 사용하십시오:

```sh
docker run --rm <image_name> <command>
```

예를 들어, `ubuntu` 이미지를 사용하여 간단한 명령어를 실행한 후 자동으로 컨테이너를 제거하려면 다음과 같이 할 수 있습니다:

```sh
docker run --rm ubuntu echo "Hello, World!"
```

이 명령어는 `ubuntu` 이미지를 기반으로 컨테이너를 실행하고, "Hello, World!"를 출력한 후 자동으로 컨테이너를 제거합니다.


