# **`Dockerfile 구조`**
Dockerfile은 **Docker 이미지**를 생성하기 위한 **명령어와 설정 파일**입니다. 

각 명령어는 이미지의 **계층(layer)**을 형성하며, Docker는 이를 조합해 최종 이미지를 만듭니다.

---

## ️ **Dockerfile 기본 구조와 주요 명령어**  

### **1. `FROM` - 베이스 이미지 지정**  
이미지 생성의 **기반**이 되는 이미지를 선택합니다.  
```dockerfile
FROM ubuntu:20.04
```
> 💡 **예시:** Node.js 프로젝트라면 `FROM node:18-alpine` 등을 사용합니다.

---

### **2. `LABEL` - 메타데이터 추가**  
이미지에 **설명, 버전, 작성자** 등의 메타데이터를 추가합니다.  
```dockerfile
LABEL maintainer="user@example.com" \
      version="1.0" \
      description="Sample Dockerfile"
```

---

### **3. `RUN` - 명령어 실행 및 설치**  
이미지 빌드 중 **패키지 설치**나 **명령어 실행**을 수행합니다.  
```dockerfile
RUN apt-get update && apt-get install -y nginx
```
> ⚡ **Tip:** 여러 `RUN` 명령어는 한 줄로 연결하여 이미지 크기를 줄입니다.  
> ```dockerfile
> RUN apt-get update && apt-get install -y python3 python3-pip && apt-get clean
> ```

---

### **4. `COPY` & `ADD` - 파일 복사**  
- `COPY`: 단순히 **호스트 → 컨테이너**로 파일/디렉토리 복사  
- `ADD`: `COPY` 기능 + **압축 파일 해제** 및 **URL 다운로드** 지원  
```dockerfile
COPY ./app /usr/src/app
ADD https://example.com/app.tar.gz /usr/src/app/
```

---

### **5. `WORKDIR` - 작업 디렉토리 설정**  
명령어 실행의 **기본 경로**를 지정합니다.  
```dockerfile
WORKDIR /usr/src/app
```
> 💡 `WORKDIR`를 사용하면 `RUN cd /path` 대신 깔끔한 경로 지정이 가능합니다.

---

### **6. `ENV` - 환경 변수 설정**  
컨테이너 내부에서 사용할 **환경 변수**를 설정합니다.  
```dockerfile
ENV NODE_ENV=production
ENV PORT=3000
```

---

### **7. `EXPOSE` - 포트 지정**  
컨테이너가 **외부와 통신**하는 포트를 정의합니다.  
```dockerfile
EXPOSE 80
EXPOSE 443
```
> 📢 실제 포트 연결은 `docker run -p 8080:80` 명령에서 이루어집니다.

---

### **8. `CMD` - 컨테이너 시작 시 실행할 명령**  
컨테이너가 시작될 때 **기본적으로 실행**할 명령을 지정합니다.  
```dockerfile
CMD ["node", "app.js"]
```
> ⚡ **특징:**  
> - Docker 실행 시 다른 명령어를 전달하면 `CMD`는 **무시**됩니다.  
> - **쉘 형식**: `CMD node app.js`  
> - **Exec 형식(권장)**: `CMD ["node", "app.js"]`

---

### **9. `ENTRYPOINT` - 컨테이너 기본 실행 명령**  
`CMD`와 유사하지만, **항상 실행**됩니다. `CMD`는 인자로 전달됩니다.  
```dockerfile
ENTRYPOINT ["python3", "script.py"]
CMD ["--help"]
```
> ⚡ **특징:**  
> - `ENTRYPOINT`는 **무조건 실행**됩니다.  
> - `CMD`는 `ENTRYPOINT`의 **기본 인자** 역할을 합니다.

---

### **10. `VOLUME` - 볼륨 마운트**  
컨테이너와 호스트 간의 **데이터 공유**를 위해 볼륨을 생성합니다.  
```dockerfile
VOLUME ["/data"]
```
> 💡 애플리케이션 로그나 데이터베이스 데이터를 지속시키는 데 사용됩니다.

---

### **11. `USER` - 실행할 사용자 지정**  
컨테이너 실행 시 사용할 **사용자 계정**을 지정합니다.  
```dockerfile
USER appuser
```
> ⚡ 보안 강화를 위해 루트 계정보다는 별도의 사용자를 지정하는 것이 좋습니다.

---

### **12. `HEALTHCHECK` - 상태 확인**  
컨테이너의 **정상 작동 여부**를 검사하는 명령어를 설정합니다.  
```dockerfile
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
```

---

### **13. `ARG` - 빌드 시 인자 전달**  
**빌드 시점에만** 사용되는 변수입니다.  
```dockerfile
ARG APP_VERSION=1.0
RUN echo "Building version $APP_VERSION"
```
> 📦 사용 예:  
> ```bash
> docker build --build-arg APP_VERSION=2.0 -t myapp:2.0 .
> ```

---

## **Dockerfile 예제** (Node.js 앱)

```dockerfile
# 1. 베이스 이미지 지정
FROM node:18-alpine

# 2. 메타데이터 추가
LABEL maintainer="you@example.com"

# 3. 작업 디렉토리 생성
WORKDIR /usr/src/app

# 4. 의존성 복사 및 설치
COPY package*.json ./
RUN npm install --production

# 5. 애플리케이션 소스 복사
COPY .. .

# 6. 애플리케이션이 사용하는 포트 노출
EXPOSE 3000

# 7. 애플리케이션 시작
CMD ["node", "server.js"]
```

---

# **Dockerfile 빌드 및 실행**

### **이미지 빌드**
```bash
docker build -t mynodeapp .
```

### **컨테이너 실행**
```bash
docker run -d -p 3000:3000 mynodeapp
```

---

## **요약: Dockerfile 구조**  
1️⃣ **FROM**: 베이스 이미지  
2️⃣ **LABEL**: 메타데이터  
3️⃣ **RUN**: 설치 및 설정 명령  
4️⃣ **COPY/ADD**: 파일 복사  
5️⃣ **WORKDIR**: 작업 경로  
6️⃣ **ENV**: 환경 변수  
7️⃣ **EXPOSE**: 포트 노출  
8️⃣ **CMD**: 기본 실행 명령  
9️⃣ **ENTRYPOINT**: 기본 실행 명령  
🔟 **VOLUME**: 데이터 저장소  
1️⃣1️⃣ **USER**: 실행 사용자  
1️⃣2️⃣ **HEALTHCHECK**: 상태 점검  
1️⃣3️⃣ **ARG**: 빌드 인자
---
