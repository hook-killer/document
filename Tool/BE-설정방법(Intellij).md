# Intellij Configuration

## 설치 필요 플러그인

- LomBok
- OpenJDK 17

## Annotation Processor

1. Mac을 기준으로 `Intellij IDEA` → `Settings`클릭
2. `Build, Execution, Deployment` → `CompilerAnnotation Processors`
3. `Enable annotation processing` 체크표시

<aside>
💡 해당 방법은 `MacOS`, `Intellij IDEA` 기준입니다. 
`VSCode` React설정은 알아서 하세욧!

</aside>

---

## 세팅순서

1. Git Clone

   ```bash
   git clone https://github.com/hook-killer/back-end.git
   ```

2. VMOptions 설정
   ```java
   -Dnaver.storage.access-key=yXOrGQFmk7BfURVyU5Dw
   -Dnaver.storage.secret-key=bv0q1CSqr8kPEjZmyTA5qSreGz4FWUTJtJFObiO6
   -Dspring.mail.username=${자신의 구글 계정을 기록하세요}
   -Dspring.mail.password=${자신의 구글 앱 패스워드를 기록하세요}
   -Dpapago.header.X-NCP-APIGW-API-KEY-ID=ybakk0x770
   -Dpapago.header.X-NCP-APIGW-API-KEY=DQxOkhtAKEqN61A5vGrp5mYW1j66Qi7vR0R7fJh5
   ```
