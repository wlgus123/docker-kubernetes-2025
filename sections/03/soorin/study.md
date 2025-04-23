# 바인드 마운트
사용법 : docker run -v (호스트머신 경로):(도커 이미지 내 경로)

# 다른 볼륨 결합 & 병합하기
docker 볼륨은 더 자세한 경로에 대해서 우선한다.

사용법 : docker run -v /Users/soorink/documents/docker_test/app:/app -v /app/node_modules

위와 같이 사용한다면, 바인드 마운트에서는 /app 까지만 사용하고, 뒤 익명볼륨은 /app/node_modules 으로
바인드 마운트에서 사용하는 /app 보다 더 자세한 경로이기 때문에, 익명볼륨인 /app/node_modules 가 우선시 되어서 
/app 아래 다른 부분은 바뀌어도, 익명 볼륨인 /app/node_modules는 변경되지 않는다.