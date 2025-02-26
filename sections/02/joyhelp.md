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


## [Docker 명령어 요약] 

`docker run`과 `docker start`는 모두 Docker 컨테이너를 실행하는 명령어이지만, **용도와 동작 방식**에 차이가 있습니다. 아래에서 자세히 설명할게요.

---

## 🚀 **1. `docker run`**  
### ✅ **설명:**  
- **새로운 컨테이너를 생성하고 실행**합니다.  
- 지정한 이미지로부터 컨테이너를 **처음부터** 시작합니다.  
- 이미지가 로컬에 없으면, **자동으로 다운로드**한 후 실행합니다.  
- `docker run`은 내부적으로 다음과 같은 과정을 수행합니다:
  1. `docker create` (컨테이너 생성)  
  2. `docker start` (컨테이너 시작)  
  3. `docker attach` (필요할 경우 표준 입출력 연결)

### 📝 **기본 사용법:**
```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

### 💡 **예제:**
```bash
docker run -d -p 8080:80 nginx
```
> 위 명령어는 `nginx` 이미지로부터 새로운 컨테이너를 생성하고, 포트를 매핑하여 백그라운드(`-d`)에서 실행합니다.

### ⚡ **주요 옵션:**
- `-d`: 백그라운드에서 실행 (detached mode)  
- `-p`: 포트 매핑 (`호스트:컨테이너`)  
- `--name`: 컨테이너 이름 지정  
- `-v`: 볼륨 마운트  
- `-e`: 환경 변수 설정  

---

## 🔄 **2. `docker start`**  
### ✅ **설명:**  
- **이미 생성된 컨테이너를 시작**합니다.  
- 컨테이너가 **중지(`stopped`) 상태**일 때 사용합니다.  
- 컨테이너가 처음 실행되었을 때의 **설정 및 옵션을 재사용**합니다.  
- `docker run`과 달리 새로 이미지를 다운로드하거나 컨테이너를 생성하지 않습니다.

### 📝 **기본 사용법:**
```bash
docker start [OPTIONS] CONTAINER
```

### 💡 **예제:**
```bash
docker start mycontainer
```
> 이름이 `mycontainer`인 기존 컨테이너를 시작합니다.

### ⚡ **주요 옵션:**
- `-a`: 컨테이너의 표준 출력과 연결 (attach)  
- `-i`: 입력을 활성화 (interactive)

---

## 🔍 **3. 주요 차이점 비교**

| 🏷️ **기능**           | 🚀 **`docker run`**                      | 🔄 **`docker start`**                 |
|-------------------|--------------------------------------|-----------------------------------|
| 🛠️ **용도**         | 새 컨테이너 생성 후 실행                 | 기존에 생성된 컨테이너를 재실행        |
| 💾 **이미지 다운로드** | 로컬에 이미지가 없으면 다운로드           | 다운로드 없음 (이미 생성된 컨테이너 사용) |
| ⚙️ **설정 재정의**    | 새로운 옵션 및 설정 적용 가능              | 초기 설정을 그대로 사용               |
| 🔄 **컨테이너 상태**  | **항상 새 컨테이너** 생성                | **중지된 컨테이너**를 재시작           |
| 🖇️ **명령어 실행**   | 새로운 명령어 전달 가능                  | 기존 시작 명령어만 사용 (`CMD`, `ENTRYPOINT`) |
| 🔄 **중지된 컨테이너 재실행** | ❌ 새로운 컨테이너 생성 필요            | ✅ 한 줄로 간단하게 재실행 가능        |

---

## 🧪 **4. 예제 시나리오로 이해하기**  

### 🎬 **Step 1: `docker run`으로 새 컨테이너 생성**
```bash
docker run --name webserver -d -p 8080:80 nginx
```
- `nginx` 이미지를 기반으로 `webserver`라는 이름의 컨테이너를 생성하고 실행합니다.

---

### 🛑 **Step 2: 컨테이너 중지**
```bash
docker stop webserver
```
- `webserver` 컨테이너를 중지합니다.

---

### 🔄 **Step 3: `docker start`로 동일 컨테이너 재시작**
```bash
docker start webserver
```
- **동일한 설정과 상태로 컨테이너를 다시 시작**합니다.  
- `docker run`을 다시 사용할 필요 없이 간단히 재실행됩니다.

---

## ⚡ **5. 실전 활용 팁**  

| 🌟 **요구사항**                  | 🏃 **추천 명령어**          |
|------------------------------|----------------------|
| 새로운 앱 컨테이너 실행           | `docker run`         |
| 한 번 중지한 컨테이너 재시작       | `docker start`       |
| 옵션 변경 후 새 컨테이너 실행      | `docker run` (옵션 수정) |
| 재부팅 후 이전 컨테이너 복구 실행   | `docker start`       |

---

## 📝 **6. 한 눈에 정리: 핵심 요약**  
- ⚡ `docker run`: **처음부터 새 컨테이너 생성** + 실행  
- 🔄 `docker start`: **기존 컨테이너 재시작** (빠르고 설정 유지)  
- 🚀 새 기능 추가나 설정 변경 시 `run` 사용  
- 🔄 단순한 재시작은 `start` 사용  

---


```
# Docker 기본 명령어

옵션	내용
-a attach (Docker start 옵션)
-d	detached mode 
-p	포트 포워딩 HOST:CONTAINER
-v	디렉토리 마운팅 HOST:CONTAINER
-e	컨테이너에서 사용할 환경변수 설정
-name	컨테이너 이름
-rm	컨테이너 제거
-rmi	이미지 제거
-it	터미널 입력

# Docker 이미지 Pull
docker pull [이미지 이름]

# Docker 이미지 확인
- docker image ls

# 새로운 이미지 태그 지정 (alpine:3.10 -> alpine:custom_3.10)
docker image tag alpine:3.10 alpine:custom_3.10

# 실행중인 컨테이너 확인
docker ps

# 종료된 컨테이너까지 확인
docker ps -a

```

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
