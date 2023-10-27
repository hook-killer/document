# BackEnd

> [!NOTE]
> 해당 페이지에서는 Docker파일의 역할과, Jenkins에서의 빌드 작업을 기록되어있습니다.

## Dockerfile

> [!WARNING]
> ENTRYPOINT의 경우에는 가독성을 위해 계행을 추가하였지만, 한줄로 사용해야합니다.

> [!NOTE]
> ARG의 경우 Image빌드 과정에서 값을 주입하기 위해서 사용
> Hostname의 경우 Pinpoint에서 동일한 이름이 들어가면 안되기 떄문에 HostName을 운영중인 큐브에서 수신받음.

```docker
FROM  --platform=linux/amd64 openjdk:17-alpine
ARG JAR_FILE=build/libs/app.jar

RUN mkdir /pinpoint

COPY ${JAR_FILE} /
COPY pinpoint/ /pinpoint/

ARG SPRING_PROFILE
ARG PAPAGO_ID
ARG PAPAGO_KEY
ARG MAIL_ID
ARG MAIL_PW
ARG NAVER_ACCESS_KEY
ARG NAVER_SECRET_KEY
ARG KAKAO_CLIENT_ID
ARG KAKAO_CLIENT_SECRET
ARG KAKAO_APP_ID
ARG KAKAO_ADMIN_KEY

ENV SPRING_PROFILE=${SPRING_PROFILE}
ENV PAPAGO_ID=${PAPAGO_ID}
ENV PAPAGO_KEY=${PAPAGO_KEY}
ENV MAIL_ID=${MAIL_ID}
ENV MAIL_PW=${MAIL_PW}
ENV NAVER_ACCESS_KEY=${NAVER_ACCESS_KEY}
ENV NAVER_SECRET_KEY=${NAVER_SECRET_KEY}
ENV KAKAO_CLIENT_ID=${KAKAO_CLIENT_ID}
ENV KAKAO_CLIENT_SECRET=${KAKAO_CLIENT_SECRET}
ENV KAKAO_APP_ID=${KAKAO_APP_ID}
ENV KAKAO_ADMIN_KEY=${KAKAO_ADMIN_KEY}
ENV HOST_NAME=$(hostname)

USER root
EXPOSE 8080

ENTRYPOINT [
  "sh",
  "-c",
  "java \
    -Dspring.profiles.active=$SPRING_PROFILE \
    -Djava.security.egd=file:/dev/./urandom \
    -Dpapago.header.X-NCP-APIGW-API-KEY-ID=$PAPAGO_ID \
    -Dpapago.header.X-NCP-APIGW-API-KEY=$PAPAGO_KEY \
    -Dspring.mail.username=$MAIL_ID \
    -Dspring.mail.password=$MAIL_PW \
    -Dnaver.storage.access-key=$NAVER_ACCESS_KEY \
    -Dnaver.storage.secret-key=$NAVER_SECRET_KEY \
    -Doauth2.kakao.client-id=$KAKAO_CLIENT_ID \
    -Doauth2.kakao.client-secret=$KAKAO_CLIENT_SECRET \
    -Doauth2.kakao.app-id=$KAKAO_APP_ID \
    -Doauth2.kakao.admin-key=$KAKAO_ADMIN_KEY \
    -javaagent:/pinpoint/pinpoint-bootstrap-2.5.2.jar \
    -Dpinpoint.applicationName=be-app \
    -Dpinpoint.agentId=$HOST_NAME \
    -jar /app.jar"]
```

## CD - 빌드 과정

```bash
# Spring Application 빌드
chmod +x ./gradlew
./gradlew build

# Docker의 생성된 Image확인
echo "Spring Build And Docker Image Push By Ncloud Cloud Registry Start"
sudo docker images
ls -al

# Naver Cloud Platform의 Docker Container Registry로 로그인
echo "Docker Container Registry 로그인"
sudo docker login ey1cp5gn.kr.private-ncr.ntruss.com -u yXOrGQFmk7BfURVyU5Dw -p bv0q1CSqr8kPEjZmyTA5qSreGz4FWUTJtJFObiO6

# Docker이미지 빌드
echo "Docker Image Build"
sudo docker build \
  --build-arg SPRING_PROFILE=dev \
  --build-arg PAPAGO_ID=ybakk0x770 \
  --build-arg PAPAGO_KEY=DQxOkhtAKEqN61A5vGrp5mYW1j66Qi7vR0R7fJh5 \
  --build-arg MAIL_ID=dev.jong1@gmail.com \
  --build-arg MAIL_PW=tdkwossxvvujtzvo \
  --build-arg NAVER_ACCESS_KEY=yXOrGQFmk7BfURVyU5Dw \
  --build-arg NAVER_SECRET_KEY=bv0q1CSqr8kPEjZmyTA5qSreGz4FWUTJtJFObiO6 \
  --build-arg KAKAO_CLIENT_ID=2899d52a6041d90ae4c9b9645a7c291d \
  --build-arg KAKAO_CLIENT_SECRET=9nI9kKWteLr6kMH50gXTs8HbJ5qvRVq4 \
  --build-arg KAKAO_APP_ID=d13220db279c497ef47b29323ceb2e20 \
  --build-arg KAKAO_ADMIN_KEY=262a014744528270dde2033856929ddb \
  -t ey1cp5gn.kr.private-ncr.ntruss.com/hook-killer-be:latest .


# Naver Cloud Platform의 Docker Container Registry로 Image Push
echo "Docker Image Naver Container Registry Push Start"
sudo docker push ey1cp5gn.kr.private-ncr.ntruss.com/hook-killer-be:latest
echo "Docker Image Naver Container Registry Push Finish"
```

## CD - 배포 과정

```bash
# Naver Cloud Platform의 Docker Container Registry로 로그인
echo "Docker Login"
sudo docker login ey1cp5gn.kr.private-ncr.ntruss.com -u yXOrGQFmk7BfURVyU5Dw -p bv0q1CSqr8kPEjZmyTA5qSreGz4FWUTJtJFObiO6

# Naver Cloud Platform의 Docker Container Registry에서 Pull한 BE 이미지 Pull
echo "Docker Container Registry Image Pull"
sudo docker pull ey1cp5gn.kr.private-ncr.ntruss.com/hook-killer-be:latest

# 기존에 사용중이던 Application Container Kill
echo "Docker Container Kill And Remove"
sudo docker stop hook-killer-be
sudo docker rm hook-killer-be

# Naver Cloud Platform에서 Pull받은 BE이미지 컨테이너로 제작 및 실행
echo "Docker Container Run Start"
sudo docker run -e HOST_NAME=$(hostname)  -d -p 8080:8080 --name hook-killer-be ey1cp5gn.kr.private-ncr.ntruss.com/hook-killer-be:latest

# 이미지 실행결과 확인
echo "Docker Process Run List"
sudo docker ps
```
