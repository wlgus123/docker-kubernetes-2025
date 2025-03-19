## 88. 볼륨으로 MongoDB에 데이터 지속성 추가하기

1. docker run --name mongodb --rm -d --network goals-net mongo
=> docker stop시 이전 데이타 소실

2. volume 지정
docker run --name mongodb -v data:/data/db --rm -d --network goals-net mongo

3. 환경변수 지정(USERNAME/PASSWORD)
docker run --name mongodb -v data:/data/db --rm -d --network goals-net -e MONGO_INITDB_ROOT_USERNAME=max -e MONGO_INITDB_ROOT_PASSWORD=secret mongo

## 90.  (바인드 마운트로) React 컨테이너에 대한 라이브 소스 코드 업데이트하기
