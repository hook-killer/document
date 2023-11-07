# README

> 프로젝트가 종료될 때 쯤엔 해당 Repository를 Public으로 공개할 예정입니다.

## Index

- [README](#readme)
  - [Index](#index)
  - [1️⃣프로젝트 소개](#1️⃣프로젝트-소개)
    - [1. 개요](#1-개요)
    - [2. 멤버](#2-멤버)
    - [3. 일정](#3-일정)
  - [2️⃣아키텍쳐](#2️⃣아키텍쳐)
  - [3️⃣기술스택](#3️⃣기술스택)
    - [1. FrontEnd](#1-frontend)
    - [2. BackEnd](#2-backend)
    - [3. Service](#3-service)
    - [4. DB](#4-db)
    - [5. CI/CD](#5-cicd)
    - [6. Tool](#6-tool)
  - [4️⃣ERD](#4️⃣erd)
  - [5️⃣와이어프레임](#5️⃣와이어프레임)
  - [6️⃣개발 및 이슈, 회고](#6️⃣개발-및-이슈-회고)
    - [종원](#종원)
    - [세환](#세환)
    - [진석](#진석)
    - [근우](#근우)
    - [정훈](#정훈)
    - [재운](#재운)
  - [7️⃣부하테스트 기록 이미지](#7️⃣부하테스트-기록-이미지)
  - [8️⃣Project Demo](#8️⃣project-demo)
    - [Demo 기능별 시연 영상](#demo-기능별-시연-영상)
    - [Demo 총 시연 영상](#demo-총-시연-영상)
  - [9️⃣발표 영상](#9️⃣발표-영상)

## 1️⃣프로젝트 소개

### 1. 개요

![LOGO](https://github.com/hook-killer/.github/blob/main/profile/asset/Logo.png?raw=true)

> [!NOTE]  
> 거리가 가까운 동북아 3국인 중국, 한국, 일본 세개국의 더 돈독한 화합과 교류를 위한 커뮤니티를 만들어보고 싶어 제작해본 프로젝트입니다.

### 2. 멤버

<table>
 <tr>
    <td align="center"><a href="https://github.com/donsonioc2010"><img src="https://avatars.githubusercontent.com/donsonioc2010" width="140px;" alt=""></a></td>
    <td align="center"><a href="https://github.com/bongsh0112"><img src="https://avatars.githubusercontent.com/bongsh0112" width="140px;" alt=""></a></td>
    <td align="center"><a href="https://github.com/wooni89"><img src="https://avatars.githubusercontent.com/u/77907190?v=4" width="140px;" alt=""></a></td>
    <td align="center"><a href="https://github.com/lljh1992"><img src="https://avatars.githubusercontent.com/u/134458007?v=4" width="140px;" alt=""></a></td>
    <td align="center"><a href="https://github.com/kwchoi11"><img src="https://avatars.githubusercontent.com/u/131943335?v=4" width="140px;" alt=""></a></td>
    <td align="center"><a href="https://github.com/lgsok00"><img src="https://avatars.githubusercontent.com/u/80325051?v=4" width="140px;" alt=""></a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/donsonioc2010"><b>종원</b></a></td>
    <td align="center"><a href="https://github.com/bongsh0112"><b>세환</b></a></td>
    <td align="center"><a href="https://github.com/wooni89"><b>재운</b></a></td>
    <td align="center"><a href="https://github.com/lljh1992"><b>정훈</b></a></td>
    <td align="center"><a href="https://github.com/kwchoi11"><b>근우</b></a></td>
    <td align="center"><a href="https://github.com/lgsok00"><b>진석</b></a></td>
  </tr>
</table>

### 3. 일정

> [!NOTE]
>
> `23년 09월 27일`에 시작하였으며, 설계, 구축, 개발, 시연 총 4개의 단계를 진행하였습니다.
> 이후 `23년 10월 27일` 개발을 완료하였습니다.

## 2️⃣아키텍쳐

> [!NOTE]  
> Proxy Server를 제외한 모든 인프라를 Private Zone에서 구축을 해보고자 하였습니다.
>
> 인프라에서 실제로 공개된 영역은 Pinpoint, Jenkins, API Server, Nginx Server만 Public 하게 노출이 된 구상도입니다.

![Architecture Map](./Architecture/Hook_killer%20Architecture%20Final.png)

## 3️⃣기술스택

### 1. FrontEnd

![NGINX](https://img.shields.io/badge/NGINX-009639?style=flat&logo=NGINX&logoColor=white)
![React](https://img.shields.io/badge/React-v.18-61DAFB?style=flat&logo=React&logoColor=white)
![ReactRouter](https://img.shields.io/badge/ReactRouter-v.6-CA4245?style=flat&logo=React_Router&logoColor=white)
![ReactQuery](https://img.shields.io/badge/ReactQuery-v.6-FF4154?style=flat&logo=React_Query&logoColor=white)
![Axios](https://img.shields.io/badge/Axios-5A29E4?style=flat&logo=Axios&logoColor=white)
![i18next](https://img.shields.io/badge/i18next-26A69A?style=flat&logo=i18next&logoColor=white)
![Quill](https://img.shields.io/badge/React-Quill-green)
![MUI](https://img.shields.io/badge/MUI-007FFF?style=flat&logo=MUI&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-v.5-7952B3?style=flat&logo=Bootstrap&logoColor=white)
![ENV](https://img.shields.io/badge/.env-ECD53F?style=flat&logo=.env&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-E7DF1E?style=flat&logo=javascript&logoColor=white)
![HTML5](https://img.shields.io/badge/html-5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/css-3-1572B6?style=flat&logo=css3&logoColor=white)

### 2. BackEnd

![JDK17](https://img.shields.io/badge/Java-v.17-CC0000?style=flat&logo=OpenJDK&logoColor=white)
![Gradle](https://img.shields.io/badge/Gradle-v.8-02303A?style=flat&logo=Gradle&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring-Boot_v.3-6DB33F?style=flat&logo=Spring-Boot&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/Spring-Data_JPA-6DB33F?style=flat&logo=Spring&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring-Security-6DB33F?style=flat&logo=Spring-Security&logoColor=white)
![Spring Cloud](https://img.shields.io/badge/Spring-Cloud-E50914?style=flat&logo=Netflix&logoColor=white)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-v.3-005F0F?style=flat&logo=Thymeleaf&logoColor=white)
![jwt](https://img.shields.io/badge/JWT-000000?style=flat&logo=jsonwebtokens&logoColor=white)

### 3. Service

![NCP](https://img.shields.io/badge/NCP-Load_Balancer-03C75A?style=flat&logo=Naver&logoColor=white)
![NCP](https://img.shields.io/badge/NCP-Container_Registry-03C75A?style=flat&logo=Naver&logoColor=white)
![NCP](https://img.shields.io/badge/NCP-Global_DNS-03C75A?style=flat&logo=Naver&logoColor=white)
![NCP](https://img.shields.io/badge/NCP-Object_Storage-03C75A?style=flat&logo=Naver&logoColor=white)
![NCP](https://img.shields.io/badge/NCP-Server-03C75A?style=flat&logo=Naver&logoColor=white)
![NCP](https://img.shields.io/badge/NCP-NAT_Gateway-03C75A?style=flat&logo=Naver&logoColor=white)
![NCP](https://img.shields.io/badge/NCP-Papago_API-03C75A?style=flat&logo=Naver&logoColor=white)
![NCP](https://img.shields.io/badge/NCP-Effective_Log_Search_&_Analytics-03C75A?style=flat&logo=Naver&logoColor=white)
![NCP](https://img.shields.io/badge/Naver-Docker_Pinpoint_v2.5.2-03C75A?style=flat&logo=Naver&logoColor=white)
![Kakao](https://img.shields.io/badge/Kakao-OAuth-FFCD00?style=flat&logo=KakaoTalk&logoColor=white)
![Google](https://img.shields.io/badge/Google-OAuth-4285F4?style=flat&logo=Google&logoColor=white)

### 4. DB

![MySQL](https://img.shields.io/badge/MySQL-v.8.0.33-4479A1?style=flat&logo=MySQL&logoColor=white)

### 5. CI/CD

![Github Actions](https://img.shields.io/badge/Github_Actions-2088FF?style=flat&logo=Github-Actions&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=Docker&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat&logo=Jenkins&logoColor=white)

### 6. Tool

![Github](https://img.shields.io/badge/GitHub-181717?style=flat&logo=GitHub&logoColor=white)
![Intellij IDEA](https://img.shields.io/badge/IntelliJ-000000?style=flat&logo=IntelliJ-IDEA&logoColor=white)
![VSCode](https://img.shields.io/badge/VSCode-007ACC?style=flat&logo=Visual-Studio-Code&logoColor=white)
![Slack](https://img.shields.io/badge/Slack-4A154B?style=flat&logo=Slack&logoColor=white)
![Notion](https://img.shields.io/badge/Notion-000000?style=flat&logo=Notion&logoColor=white)
![PostMan](https://img.shields.io/badge/Postman-FF6C37?style=flat&logo=Postman&logoColor=white)

## 4️⃣ERD

[![ERD](./ERD/ERD_v231027.png)](https://dbdocs.io/donsonioc2010/Hook_killer)

## 5️⃣와이어프레임

> [!WARNING]  
> 많은 양의 이미지 이다보니 위키 링크를 첨부하였습니다.

- [와이어프레임 위키 링크](https://github.com/hook-killer/document/wiki/01.-WireFrame)

## 6️⃣개발 및 이슈, 회고

> 각자 개발하면서 어떤 생각을 하는지 꾸준히 기록해보세요 😃

> 개발하면서 발생한 이슈 또는 개발관련 문제들에 대해서 기록하고 링크를 걸어보세요😃

### 종원

- [팀 결성, 프로젝트의 시작과 개요](https://devjong12.tistory.com/109)
- [PinPoint의 설치 과정](https://devjong12.tistory.com/110)
- [Container로 빌드되는 Spring Boot Application에 PinPoint Agent 적용기](https://devjong12.tistory.com/111)

### 세환

### 진석

### 근우

### 정훈

### 재운

## 7️⃣부하테스트 기록 이미지

> 위키로 정리하고 위키 링크 기록하기

## 8️⃣Project Demo

> [!INFO]
> 실제 전부 개발했던 시연 영상입니다.

### Demo 기능별 시연 영상

> [!WARNING]  
> 많은 양의 이미지 이다보니 위키 링크를 첨부하였습니다.
> 양해 부탁드립니다.

- [프로젝트 데모 시연 위키 링크](https://github.com/hook-killer/document/wiki/Project-Demo)

### Demo 총 시연 영상

[![Project Demo Youtube](http://img.youtube.com/vi/TCDPdvttXfw/0.jpg)](https://youtu.be/TCDPdvttXfw)

## 9️⃣발표 영상

[![Project Result Youtube](http://img.youtube.com/vi/crN5Hoiw8ds/0.jpg)](https://youtu.be/crN5Hoiw8ds)
