# FrontEnd

> [!NOTE]
> 해당 페이지에서는 Docker파일의 역할과, Jenkins에서의 빌드 작업을 기록되어있습니다.

## Dockerfile

> [!NOTE]
> 해당 Dockerfile의 경우에는 2단계에 걸쳐서 구현함.
> Builder로 React를 빌드하며, 빌드한 파일을 nginx컨테이너에서 수신받아 실행한다.

```dockerfile
FROM node:latest as builder

WORKDIR /app
COPY    package.json package-lock.json ./
ENV     NODE_ENV=prod
RUN     npm install
COPY    . .
RUN     npm run build


FROM nginx:latest
COPY --from=builder /app/build /app
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## CD - 빌드과정

```bash
echo "Naver Cloud Platform Docker Container Registry Login"
sudo docker login ey1cp5gn.kr.private-ncr.ntruss.com -u yXOrGQFmk7BfURVyU5Dw -p bv0q1CSqr8kPEjZmyTA5qSreGz4FWUTJtJFObiO6

echo "Docker Image Build"
sudo docker build -t ey1cp5gn.kr.private-ncr.ntruss.com/hook-killer-fe .

echo "Naver Cloud Platform Docker Container Registry Image Push"
sudo docker push ey1cp5gn.kr.private-ncr.ntruss.com/hook-killer-fe:latest
```

## CD - 배포과정

```bash
echo "Naver Cloud Platform Docker Container Registry Login"
sudo docker login ey1cp5gn.kr.private-ncr.ntruss.com -u yXOrGQFmk7BfURVyU5Dw -p bv0q1CSqr8kPEjZmyTA5qSreGz4FWUTJtJFObiO6


echo "Naver Cloud Platform Docker Container Registry Image Pull"
sudo docker pull ey1cp5gn.kr.private-ncr.ntruss.com/hook-killer-fe:latest

echo "Docker Container FrontEnd Container Kill"
sudo docker stop hook-killer-fe
sudo docker rm hook-killer-fe

echo "FrontEnd Image Run Container"
sudo docker run -d -p 80:80 --name hook-killer-fe ey1cp5gn.kr.private-ncr.ntruss.com/hook-killer-fe:latest
```
