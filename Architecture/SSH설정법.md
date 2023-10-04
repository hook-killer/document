# Server

## Index

- [Server](#server)
  - [Index](#index)
  - [개요](#개요)
  - [Server Password](#server-password)
    - [Proxy Server](#proxy-server)
    - [Application Server](#application-server)
  - [Local설정](#local설정)
  - [접속 방법](#접속-방법)

## 개요

> Proxy, Application Server 모두 `hook-key.pem`를 통해 접속한다.

## Server Password

### Proxy Server

- Password : hook1234!@#$

### Application Server

- Password : hook1234!@#$

---

## Local설정

1. PEM파일 다운로드
   1. Repository/`Architecture/hook-key.pem`을 다운로드 받는다.
   2. 다운로드 한 `pem`파일을 `~/.ssh`하위 디렉토리에 옮긴다
      1. `~/.ssh`디렉토리가 존재하지 않을 수도 있는 데, 그냥 디렉토리 생성후 옮겨도 된다.
2. 각자의 Local환경에 `~/.ssh/` 디렉토리에 `config`라는 파일 명칭으로 생성한다
   1. 파일이 존재하는 경우 최하단에 붙여 넣으면 됨.
3. 파일 권한 변경
   1. `chmod 600 ~/.ssh/config`
   2. `chmod 600 ~/.ssh/hook-key.pem`

```shell
# hook-killer-proxy-server
Host hook-killer-proxy-server
    HostName 101.79.10.6
    User root
    IdentityFile ~/.ssh/hook-key.pem

# hook-killer-application
Host hook-killer-app-server
    HostName 192.168.1.6
    User root
    IdentityFile ~/.ssh/hook-key.pem
    ProxyJump hook-killer-proxy-server
```

## 접속 방법

> 터미널에서 다음의 방법으로 접속이 가능하다.

- `ssh hook-killer-proxy-server`
- `ssh hook-killer-app-server`
