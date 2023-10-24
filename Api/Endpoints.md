# EndPoint

## 개요

> [!NOTE]
> 해당 페이지는 이 프로젝트에서 작성된 EndPoint를 기록하였습니다.

## Infra

> [!NOTE]
> Infra를 위한 Endpoint입니다

<table>
    <thead>
        <tr>
            <th>RequestMapping</th>
            <th>Method</th>
            <th>Mapping</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td></td>
            <td><code>GET</code></td>
            <td><code>/health</code></td>
            <td>LB<small>(Load Balancer)</small>에서 서버가 현재 살아있다는 신호를 주기 위한 전용 Endpoint</td>
        </tr>
    </tbody>
</table>

## Auth

> [!NOTE]
> 인증 및 인가, 회원정보와 관련된 EndPoint 입니다

<table>
    <thead>
        <tr>
            <th>RequestMapping</th>
            <th>Method</th>
            <th>Mapping</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=5><code>/auth</code></td>
            <td><code></code></td>
            <td><code></code></td>
            <td></td>
        </tr>
        <tr>
            <td><code>POST</code></td>
            <td><code>/register</code></td>
            <td>사용자 회원가입</td>
        </tr>
        <tr>
            <td><code>POST</code></td>
            <td><code>/login</code></td>
            <td>로그인</td>
        </tr>
        <tr>
            <td><code></code></td>
            <td><code></code></td>
            <td></td>
        </tr>
        <tr>
            <td><code></code></td>
            <td><code></code></td>
            <td></td>
        </tr>
        <tr>
            <td rowspan=4><code>/mypage</code></td>
            <td><code>GET</code></td>
            <td><code>/</code></td>
            <td>로그인한 사용자 정보 취득</td>
        </tr>
        <tr>
            <td><code>GET</code></td>
            <td><code>/mylist/{searchType}</code></td>
            <td>로그인한 사용자의 작성물 리스트 검색</td>
        </tr>
        <tr>
            <td><code>PUT</code></td>
            <td><code>/</code></td>
            <td>사용자 정보 수정</td>
        </tr>
        <tr>
            <td><code>PUT</code></td>
            <td><code>/thumnail</code></td>
            <td>사용자 썸네일 프로필 Path변경</td>
        </tr>
    </tbody>
</table>

### 사용자 회원가입

#### Request Example

```shell
curl --location 'http://localhost:8080/auth/register' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email" : "test@test.com",
    "nickName" : "적셔",
    "password" : "11111111",
    "role" : "USER"
}'
```

#### Response

```shell
{
    "createAt": "2023-10-17 08:55:42",
    "updateAt": "2023-10-17 08:55:42",
    "id": 3,
    "email": "test@test.com",
    "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
    "nickName": "적셔",
    "thumbnail": null,
    "role": "USER",
    "oauthInfo": null,
    "status": null,
    "loginType": "DEFAULT"
}
```

### 로그인

#### Request Example

```shell
curl --location 'http://localhost:8080/auth/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email" : "admin@test.com",
    "password" : "11111111"
}'
```

#### Response Example

```json
{
  "token": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTMzMjc2LCJleHAiOjE2OTc1NjkyNzYsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.ys5w2OBBcScP-yWnHn9-_RDgHlvCuQU5Apkp9gK9J0YfZG2Ckueg4B-4Q5xOQTu3-hjOVtQSHgtCw-54-CU5Rg"
}
```

### 마이페이지 - 로그인한 사용자 정보

#### Request Example

```shell
curl --location 'http://localhost:8080/mypage' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAzNTQ1LCJleHAiOjE2OTc2Mzk1NDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.RjqfcRyI9JzgEDqOxWC9xk_ZBns9skxns3V0F9qdLpjl7mTyjl5CyWjQ5xa7MueXYgq2V3TFTiYqaBttizAIeA' \
--header 'language: KO'
```

#### Response Example

```json
{
  "userId": 1,
  "email": "admin@test.com",
  "thumbnail": null,
  "nickName": "관리자"
}
```

### 마이페이지 - 로그인한 사용자의 작성물 리스트 검색

#### Request Example

##### 게시물

```shell
curl --location 'http://localhost:8080/mypage/mylist/article?page=0&limit=5' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAzNTQ1LCJleHAiOjE2OTc2Mzk1NDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.RjqfcRyI9JzgEDqOxWC9xk_ZBns9skxns3V0F9qdLpjl7mTyjl5CyWjQ5xa7MueXYgq2V3TFTiYqaBttizAIeA' \
--header 'language: KO'
```

##### 댓글

```shell
curl --location 'http://localhost:8080/mypage/mylist/reply?page=0&limit=5' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAzNTQ1LCJleHAiOjE2OTc2Mzk1NDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.RjqfcRyI9JzgEDqOxWC9xk_ZBns9skxns3V0F9qdLpjl7mTyjl5CyWjQ5xa7MueXYgq2V3TFTiYqaBttizAIeA' \
--header 'language: KO'
```

##### 좋아요

```shell
curl --location 'http://localhost:8080/mypage/mylist/like?page=0&limit=5' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAzNTQ1LCJleHAiOjE2OTc2Mzk1NDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.RjqfcRyI9JzgEDqOxWC9xk_ZBns9skxns3V0F9qdLpjl7mTyjl5CyWjQ5xa7MueXYgq2V3TFTiYqaBttizAIeA' \
--header 'language: KO'
```

#### Response Example

##### 게시물

```json
[
  {
    "createAt": "2023-10-18 02:54:24",
    "updateAt": "2023-10-18 02:54:24",
    "boardId": 1,
    "articleId": 7,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "너무귀여운 우리 응애미쯔",
    "content": "<b>사랑해 ~~~~~~~알라쀼~~~</b>",
    "createdUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  },
  {
    "createAt": "2023-10-17 11:26:00",
    "updateAt": "2023-10-17 11:26:00",
    "boardId": 1,
    "articleId": 6,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "미쯔야~",
    "content": "<b>사랑해 ~~~~~~~</b>",
    "createdUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  },
  {
    "createAt": "2023-10-17 11:25:36",
    "updateAt": "2023-10-17 11:25:36",
    "boardId": 3,
    "articleId": 5,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "엘라스틴했어요",
    "content": "<p>배고파요 흑흑흑</p>",
    "createdUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  },
  {
    "createAt": "2023-10-17 11:25:20",
    "updateAt": "2023-10-17 11:25:20",
    "boardId": 2,
    "articleId": 4,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "오리엔탈",
    "content": "<div>유니티</div>",
    "createdUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  },
  {
    "createAt": "2023-10-17 11:24:56",
    "updateAt": "2023-10-17 11:24:56",
    "boardId": 2,
    "articleId": 3,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 1,
    "contentLanguage": "KO",
    "title": "근우야!!!!!",
    "content": "잘하자",
    "createdUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  }
]
```

##### 댓글

```json
[
  {
    "createAt": "2023-10-18 04:17:02",
    "updateAt": "2023-10-18 04:17:02",
    "articleId": 7,
    "replyId": 9,
    "orgReplyLanguage": "KO",
    "createUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "응애응애 너무 졸린 점심이에요~~~~"
  },
  {
    "createAt": "2023-10-17 12:05:33",
    "updateAt": "2023-10-17 12:05:33",
    "articleId": 5,
    "replyId": 8,
    "orgReplyLanguage": "EN",
    "createUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "나는 매우 슬프다"
  },
  {
    "createAt": "2023-10-17 12:05:20",
    "updateAt": "2023-10-17 12:05:20",
    "articleId": 5,
    "replyId": 7,
    "orgReplyLanguage": "KO",
    "createUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "뿌애애애애애애앵"
  },
  {
    "createAt": "2023-10-17 12:05:00",
    "updateAt": "2023-10-17 12:05:00",
    "articleId": 5,
    "replyId": 6,
    "orgReplyLanguage": "KO",
    "createUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "코드싸개가 흑흑흑"
  },
  {
    "createAt": "2023-10-17 12:04:30",
    "updateAt": "2023-10-17 12:04:30",
    "articleId": 4,
    "replyId": 5,
    "orgReplyLanguage": "KO",
    "createUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "여기 사람있어요ㅠㅠㅠㅠ"
  }
]
```

##### 좋아요

```json
[
  {
    "createAt": "2023-10-17 11:24:56",
    "updateAt": "2023-10-17 11:24:56",
    "boardId": 2,
    "articleId": 3,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 1,
    "contentLanguage": "KO",
    "title": "근우야!!!!!",
    "content": "잘하자",
    "createdUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  },
  {
    "createAt": "2023-10-17 11:25:20",
    "updateAt": "2023-10-17 11:25:20",
    "boardId": 2,
    "articleId": 4,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "오리엔탈",
    "content": "<div>유니티</div>",
    "createdUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 04:44:57",
      "updateAt": "2023-10-18 04:44:57",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  }
]
```

### 마이페이지 - 사용자 정보 수정

#### Request Example

```shell

```

#### Response Example

```json

```

### 마이페이지 - 로그인한 사용자 정보

#### Request Example

```shell
curl --location --request PUT 'http://localhost:8080/mypage' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjA3MTA1LCJleHAiOjE2OTc2NDMxMDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.eV0-PuDE1og7c4Q7mJAM_-CGDCKIrhiCiO2BymuBXEV8d_-6WLwmXBRKYPbYFlcABdx8hKMEu7DpT8sHndq99Q' \
--header 'language: KO' \
--header 'Content-Type: application/json' \
--data '{
    "password" : "2222",
    "nickName" : "야 뽕세환~"
}'
```

#### Response Example

```json
{
  "result": true,
  "message": "수정이 완료되었습니다."
}
```

### 마이페이지 - 사용자 썸네일 프로필 Path변경

#### Request Example

##### Path가 존재하는 경우

```shell
curl --location --request PUT 'http://localhost:8080/mypage/thumnail' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjA3MTA1LCJleHAiOjE2OTc2NDMxMDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.eV0-PuDE1og7c4Q7mJAM_-CGDCKIrhiCiO2BymuBXEV8d_-6WLwmXBRKYPbYFlcABdx8hKMEu7DpT8sHndq99Q' \
--header 'language: KO' \
--header 'Content-Type: application/json' \
--data '{
    "thumnail": "local/abc/j.jpg"
}'
```

##### 문제가 있는 경우

```shell
curl --location --request PUT 'http://localhost:8080/mypage/thumnail' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjA3MTA1LCJleHAiOjE2OTc2NDMxMDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.eV0-PuDE1og7c4Q7mJAM_-CGDCKIrhiCiO2BymuBXEV8d_-6WLwmXBRKYPbYFlcABdx8hKMEu7DpT8sHndq99Q' \
--header 'language: KO' \
--header 'Content-Type: application/json' \
--data '{
    "thumbnail": ""
}'
```

#### Response Example

##### Path가 존재하는 경우

```json
{
  "result": true,
  "message": "수정이 완료되었습니다."
}
```

##### 문제가 있는 경우

```json
{
  "result": false,
  "message": "요청 Path가 없습니다."
}
```

## Article

> [!NOTE]
> 게시물과 연관된 Endpoint리스트 입니다.

<table>
    <thead>
        <tr>
            <th>RequestMapping</th>
            <th>Method</th>
            <th>Mapping</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=5><code>/article</code></td>
            <td><code>GET</code></td>
            <td><code>/{articleId}</code></td>
            <td>게시물 단건 조회</td>
        </tr>
        <tr>
            <td><code>GET</code></td>
            <td><code>/list/{boardId}</code></td>
            <td>게시물 리스트 조회</td>
        </tr>
        <tr>
            <td><code>POST</code></td>
            <td><code>/</code></td>
            <td>게시물 등록</td>
        </tr>
        <tr>
            <td><code>PUT</code></td>
            <td><code>/</code></td>
            <td>게시물 수정</td>
        </tr>
        <tr>
            <td><code>DELETE</code></td>
            <td><code>/{articleId}</code></td>
            <td>게시물 삭제</td>
        </tr>
        <tr>
            <td rowspan=3><code>/reply</code></td>
            <td><code>GET</code></td>
            <td><code>/{articleId}</code></td>
            <td>게시물 List조회</td>
        </tr>
        <tr>
            <td><code>POST</code></td>
            <td><code>/</code></td>
            <td>게시물 등록</td>
        </tr>
        <tr>
            <td><code>DELETE</code></td>
            <td><code>/</code></td>
            <td>댓글 삭제</td>
        </tr>
        <tr>
            <td rowspan=2><code>/article/like</code></td>
            <td><code>GET</code></td>
            <td><code>/{articleId}</code></td>
            <td>사용자가 게시물의 좋아요 판단 유무</td>
        </tr>
        <tr>
            <td><code>POST</code></td>
            <td><code>/{articleId}</code></td>
            <td>좋아요 또는 좋아요 취소</td>
        </tr>
    </tbody>
</table>

### 게시물 단건 조회

#### Request Example

```shell
curl --location 'http://localhost:8080/article/1' \
--header 'language: KO'
```

#### Response Example

```json
{
  "createAt": "2023-10-17 11:24:19",
  "updateAt": "2023-10-17 11:24:19",
  "boardId": 1,
  "articleId": 1,
  "orgArticleLanguage": "KO",
  "status": "PUBLIC",
  "likeCount": 0,
  "contentLanguage": "KO",
  "title": "제목입니다~",
  "content": "킹종원이 점령했다!",
  "createdUser": {
    "createAt": "2023-10-18 02:43:14",
    "updateAt": "2023-10-18 02:43:14",
    "id": 1,
    "email": "admin@test.com",
    "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
    "nickName": "관리자",
    "thumbnail": null,
    "role": "ADMIN",
    "oauthInfo": null,
    "status": "ACTIVE",
    "loginType": "DEFAULT"
  },
  "updatedUser": {
    "createAt": "2023-10-18 02:43:14",
    "updateAt": "2023-10-18 02:43:14",
    "id": 1,
    "email": "admin@test.com",
    "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
    "nickName": "관리자",
    "thumbnail": null,
    "role": "ADMIN",
    "oauthInfo": null,
    "status": "ACTIVE",
    "loginType": "DEFAULT"
  }
}
```

### 게시물 리스트 조회

#### Request Example

```shell
curl --location 'http://localhost:8080/article/list/1?page=0&articleLimit=3' \
--header 'language: KO'
```

#### Response Example

```json
[
  {
    "createAt": "2023-10-18 02:44:04",
    "updateAt": "2023-10-18 02:44:04",
    "boardId": 1,
    "articleId": 7,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "미쯔야~",
    "content": "<b>사랑해 ~~~~~~~</b>",
    "createdUser": {
      "createAt": "2023-10-18 02:43:14",
      "updateAt": "2023-10-18 02:43:14",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 02:43:14",
      "updateAt": "2023-10-18 02:43:14",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  },
  {
    "createAt": "2023-10-17 11:26:00",
    "updateAt": "2023-10-17 11:26:00",
    "boardId": 1,
    "articleId": 6,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "미쯔야~",
    "content": "<b>사랑해 ~~~~~~~</b>",
    "createdUser": {
      "createAt": "2023-10-18 02:43:14",
      "updateAt": "2023-10-18 02:43:14",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 02:43:14",
      "updateAt": "2023-10-18 02:43:14",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  },
  {
    "createAt": "2023-10-17 11:24:41",
    "updateAt": "2023-10-17 11:24:41",
    "boardId": 1,
    "articleId": 2,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "응애응애",
    "content": "엉엉엉어엉엉엉",
    "createdUser": {
      "createAt": "2023-10-18 02:43:14",
      "updateAt": "2023-10-18 02:43:14",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 02:43:14",
      "updateAt": "2023-10-18 02:43:14",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  }
]
```

### 게시물 등록

#### Request Example

```shell
curl --location 'http://localhost:8080/article' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTk3MDIzLCJleHAiOjE2OTc2MzMwMjMsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.c77TIvGH4Bj4WrW7YRTAWZhrFIO9ZNuz6RS8t-Skof-mGr4tYzcb5OLHK0qOMBoVdk-f2pAoeatKtHlbMpHV2w' \
--header 'Content-Type: application/json' \
--data '{
    "boardId" : 1,
    "orgArticleLanguage" : "KO",
    "title" : "미쯔야~",
    "content" : "<b>사랑해 ~~~~~~~알라쀼~~~</b>"
}'
```

#### Response Example

```text
게시물 등록이 정상적으로 완료되었습니다.
```

### 게시물 수정

#### Request Example

```shell
curl --location --request PUT 'http://localhost:8080/article' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTk3MDIzLCJleHAiOjE2OTc2MzMwMjMsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.c77TIvGH4Bj4WrW7YRTAWZhrFIO9ZNuz6RS8t-Skof-mGr4tYzcb5OLHK0qOMBoVdk-f2pAoeatKtHlbMpHV2w' \
--header 'Content-Type: application/json' \
--data '{
    "boardId" : 1,
    "articleId": 8,
    "orgArticleLanguage" : "KO",
    "title" : "미쯔야~",
    "newTitle" : "너무귀여운 우리 응애미쯔",
    "content" : "<b>사랑해 ~~~~~~~</b>"
}'
```

#### Response Example

```text
게시물 수정이 정상적으로 완료되었습니다.
```

### 게시물 삭제

#### Request Example

```shell
curl --location --request DELETE 'http://localhost:8080/article/5' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAyMDczLCJleHAiOjE2OTc2MzgwNzMsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.Bc6zvNuPTQ3PVeKOVPTm1E1PrhGIXsdJ83SniWwyI2m4MVDytjMtZjZM23Acy-5filgSzfnmB3-WfR5HaNtWeQ'
```

#### Response Example

```json
{
  "result": true,
  "message": "게시물를 정상적으로 삭제하였습니다."
}
```

### 댓글 리스트 조회

#### Request Example

```shell
curl --location 'http://localhost:8080/reply/5' \
--header 'language: KO'
```

#### Response Example

```json
[
  {
    "createAt": "2023-10-17 12:05:00",
    "updateAt": "2023-10-17 12:05:00",
    "articleId": 5,
    "replyId": 6,
    "orgReplyLanguage": "KO",
    "createUser": {
      "createAt": "2023-10-18 04:18:58",
      "updateAt": "2023-10-18 04:18:58",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "코드싸개가 흑흑흑"
  },
  {
    "createAt": "2023-10-17 12:05:20",
    "updateAt": "2023-10-17 12:05:20",
    "articleId": 5,
    "replyId": 7,
    "orgReplyLanguage": "KO",
    "createUser": {
      "createAt": "2023-10-18 04:18:58",
      "updateAt": "2023-10-18 04:18:58",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "뿌애애애애애애앵"
  },
  {
    "createAt": "2023-10-17 12:05:33",
    "updateAt": "2023-10-17 12:05:33",
    "articleId": 5,
    "replyId": 8,
    "orgReplyLanguage": "EN",
    "createUser": {
      "createAt": "2023-10-18 04:18:58",
      "updateAt": "2023-10-18 04:18:58",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "나는 매우 슬프다"
  }
]
```

### 댓글 추가

#### Request Example

```shell
curl --location 'http://localhost:8080/reply' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAyNjAxLCJleHAiOjE2OTc2Mzg2MDEsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.PfiO8SmcKnbEf8JXjXJEomXqUwZjhKA8bti8nGpkbyCwpHbtUbMXb_WvHp7HRApBuSS4VTO-hdxMphGnHboPZA' \
--header 'language: ko' \
--header 'Content-Type: application/json' \
--data '{
    "articleId" : 7,
    "orgReplyLanguage" : "KO",
    "content" : "응애응애 너무 졸린 점심이에요~~~~"
}'
```

#### Response Example

```json

```

### 댓글 삭제

#### Request Example

```shell
curl --location --request DELETE 'http://localhost:8080/reply/9' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAyNjAxLCJleHAiOjE2OTc2Mzg2MDEsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.PfiO8SmcKnbEf8JXjXJEomXqUwZjhKA8bti8nGpkbyCwpHbtUbMXb_WvHp7HRApBuSS4VTO-hdxMphGnHboPZA' \
--header 'language: ko'
```

#### Response Example

```text
삭제처리가 완료되었습니다.
```

### 좋아요 유무 판단

#### Request Example

##### 좋아요를 누르지 않은 경우

```shell
curl --location 'http://localhost:8080/article/like/1' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAyNjAxLCJleHAiOjE2OTc2Mzg2MDEsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.PfiO8SmcKnbEf8JXjXJEomXqUwZjhKA8bti8nGpkbyCwpHbtUbMXb_WvHp7HRApBuSS4VTO-hdxMphGnHboPZA' \
--header 'language: ko'
```

##### 좋아요를 누른 경우

```shell
curl --location 'http://localhost:8080/article/like/2' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAyNjAxLCJleHAiOjE2OTc2Mzg2MDEsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.PfiO8SmcKnbEf8JXjXJEomXqUwZjhKA8bti8nGpkbyCwpHbtUbMXb_WvHp7HRApBuSS4VTO-hdxMphGnHboPZA' \
--header 'language: ko'
```

#### Response Example

##### 좋아요를 누르지 않은 경우

```json
{
  "result": false,
  "message": "게시물에 좋아요를 누르지 않았습니다."
}
```

##### 좋아요를 누른 경우

```json
{
  "result": true,
  "message": "이미 게시물에 좋아요를 누르셨습니다."
}
```

### 좋아요 또는 좋아요 취소

#### Request Example

```shell
curl --location --request POST 'http://localhost:8080/article/like/1' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAzNTQ1LCJleHAiOjE2OTc2Mzk1NDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.RjqfcRyI9JzgEDqOxWC9xk_ZBns9skxns3V0F9qdLpjl7mTyjl5CyWjQ5xa7MueXYgq2V3TFTiYqaBttizAIeA' \
--header 'language: KO'
```

#### Response Example

```text
ADD_SUCCESS

or

DEL_SUCCESS
```

## Notice

> [!NOTE]
> 공지사항기능의 EndPoint 입니다.

<table>
    <thead>
        <tr>
            <th>RequestMapping</th>
            <th>Method</th>
            <th>Mapping</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=5><code>/notice</code></td>
            <td><code>GET</code></td>
            <td><code>/</code></td>
            <td>공지사항 게시물 리스트 조회</td>
        </tr>
        <tr>
            <td><code>GET</code></td>
            <td><code>/{noticeArticleId}</code></td>
            <td>공지사항 게시물 상세조회</td>
        </tr>
        <tr>
            <td><code>POST</code></td>
            <td><code>/</code></td>
            <td>공지사항 추가</td>
        </tr>
        <tr>
            <td><code>PUT</code></td>
            <td><code>/</code></td>
            <td>공지사항 게시물 수정</td>
        </tr>
        <tr>
            <td><code>DELETE</code></td>
            <td><code>/{noticeArticleId}</code></td>
            <td>공지사항 게시물 삭제</td>
        </tr>
    </tbody>
</table>

### 공지사항 게시물 리스트 조회

#### Request Example

```shell
curl --location 'http://localhost:8080/notice?page=0&articleLimit=5' \
--header 'language: KO'
```

#### Response Example

```json
[
  {
    "createAt": "2023-12-12 01:00:00",
    "updateAt": "2023-12-13 01:00:00",
    "id": 50,
    "language": "KO",
    "status": "PUBLIC",
    "createdUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": null,
    "title": "title50"
  },
  {
    "createAt": "2023-12-10 01:00:00",
    "updateAt": "2023-12-11 01:00:00",
    "id": 49,
    "language": "KO",
    "status": "PUBLIC",
    "createdUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": null,
    "title": "title49"
  },
  {
    "createAt": "2023-12-08 01:00:00",
    "updateAt": "2023-12-09 01:00:00",
    "id": 48,
    "language": "KO",
    "status": "PUBLIC",
    "createdUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": null,
    "title": "title48"
  },
  {
    "createAt": "2023-12-06 01:00:00",
    "updateAt": "2023-12-07 01:00:00",
    "id": 47,
    "language": "KO",
    "status": "PUBLIC",
    "createdUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": null,
    "title": "title47"
  },
  {
    "createAt": "2023-12-04 01:00:00",
    "updateAt": "2023-12-05 01:00:00",
    "id": 46,
    "language": "KO",
    "status": "PUBLIC",
    "createdUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-18 01:10:29",
      "updateAt": "2023-10-18 01:10:29",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": null,
    "title": "title46"
  }
]
```

### 공지사항 게시물 상세조회(단건조회)

#### Request Example

```shell
curl --location 'http://localhost:8080/notice/1' \
--header 'language: KO'
```

#### Response Example

```json
{
  "createAt": "2023-09-20 01:00:00",
  "updateAt": "2023-09-21 01:00:00",
  "id": 1,
  "language": "KO",
  "status": "PUBLIC",
  "createdUser": {
    "createAt": "2023-10-18 01:10:29",
    "updateAt": "2023-10-18 01:10:29",
    "id": 1,
    "email": "admin@test.com",
    "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
    "nickName": "관리자",
    "thumbnail": null,
    "role": "ADMIN",
    "oauthInfo": null,
    "status": "ACTIVE",
    "loginType": "DEFAULT"
  },
  "updatedUser": {
    "createAt": "2023-10-18 01:10:29",
    "updateAt": "2023-10-18 01:10:29",
    "id": 1,
    "email": "admin@test.com",
    "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
    "nickName": "관리자",
    "thumbnail": null,
    "role": "ADMIN",
    "oauthInfo": null,
    "status": "ACTIVE",
    "loginType": "DEFAULT"
  },
  "content": "content1",
  "title": "title1"
}
```

### 공지사항 추가

> [!WARNING]
> 해당 기능은 관리자 사용자의 JWT토큰이 필수입니다.

#### Request Example

```shell
curl --location 'http://localhost:8080/notice' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTMzMjc2LCJleHAiOjE2OTc1NjkyNzYsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.ys5w2OBBcScP-yWnHn9-_RDgHlvCuQU5Apkp9gK9J0YfZG2Ckueg4B-4Q5xOQTu3-hjOVtQSHgtCw-54-CU5Rg' \
--header 'Content-Type: application/json' \
--data '{
    "language" : "KO",
    "title" : "응애응애",
    "content" : "<div>응애응애응애 너무 일이 많아요</div>"
}'
```

#### Response Example

```json

```

### 공지사항 수정

> [!WARNING]
> 해당 기능은 관리자 사용자의 JWT토큰이 필수입니다.

#### Request Example

```shell
curl --location --request PUT 'http://localhost:8080/notice' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTMzMjc2LCJleHAiOjE2OTc1NjkyNzYsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.ys5w2OBBcScP-yWnHn9-_RDgHlvCuQU5Apkp9gK9J0YfZG2Ckueg4B-4Q5xOQTu3-hjOVtQSHgtCw-54-CU5Rg' \
--header 'Content-Type: application/json' \
--data '{
    "noticeArticleId" : 51,
    "language" : "KO",
    "orgTitle" : "응애응애",
    "newTitle" : "일이 너무 많다고요 ㅠㅠ",
    "orgContent" : "<div>응애응애응애 너무 일이 많아요</div>",
    "newContent" : "<div>제발 엘라스틱서치 쓸수 있게 해주세요</div>"
}'
```

#### Response Example

```json

```

### 공지사항 삭제

> [!WARNING]
> 해당 기능은 관리자 사용자의 JWT토큰이 필수입니다.

#### Request Example

```shell
curl --location --request DELETE 'http://localhost:8080/notice/50' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTMzMjc2LCJleHAiOjE2OTc1NjkyNzYsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.ys5w2OBBcScP-yWnHn9-_RDgHlvCuQU5Apkp9gK9J0YfZG2Ckueg4B-4Q5xOQTu3-hjOVtQSHgtCw-54-CU5Rg'
```

#### Response Example

```json

```

## Admin

> [!NOTE]
> 관리자 기능의 EndPoint 입니다.

<table>
    <thead>
        <tr>
            <th>RequestMapping</th>
            <th>Method</th>
            <th>Mapping</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=6><code>/admin</code></td>
            <td><code>GET</code></td>
            <td><code>/account/list/{role}</code></td>
            <td>계정리스트 조회</td>
        </tr>
        <tr>
            <td><code>GET</code></td>
            <td><code>/account/{userId}</code></td>
            <td>사용자 계정 정보 상세 조회</td>
        </tr>
        <tr>
            <td><code>GET</code></td>
            <td><code>/account/article/{userId}</code></td>
            <td>게시물의 상태와 관련없이 해당 사용자가 작성한 모든 게시물을 Page처리해서 반환한다.</td>
        </tr>
        <tr>
            <td><code>GET</code></td>
            <td><code>/account/reply/{userId}</code></td>
            <td>댓글의 상태와 관련없이 해당 사용자가 작성한 모든 댓글을 Page처리해서 반환한다.</td>
        </tr>
        <tr>
            <td><code>POST</code></td>
            <td><code>/register</code></td>
            <td>관리자 계정 추가.</td>
        </tr>
          <tr>
            <td><code>PUT</code></td>
            <td><code>/account/status</code></td>
            <td>계정 상태 변경</td>
        </tr>
    </tbody>
</table>

### 계정리스트 조회

#### Request Example

```shell
curl --location 'http://localhost:8080/admin/account/list/USER?page=0&limit=3&userStatus=ACTIVE' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTMzMjc2LCJleHAiOjE2OTc1NjkyNzYsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.ys5w2OBBcScP-yWnHn9-_RDgHlvCuQU5Apkp9gK9J0YfZG2Ckueg4B-4Q5xOQTu3-hjOVtQSHgtCw-54-CU5Rg'
```

#### Response Example

```json
[
  {
    "createAt": "2023-10-17 12:26:27",
    "updateAt": "2023-10-17 12:26:27",
    "id": 8,
    "email": "user1@test.com",
    "nickName": "사용자1",
    "createdAt": "2023-10-17T12:26:27.000+00:00"
  },
  {
    "createAt": "2023-10-17 12:26:27",
    "updateAt": "2023-10-17 12:26:27",
    "id": 9,
    "email": "user2@test.com",
    "nickName": "사용자2",
    "createdAt": "2023-10-17T12:26:27.000+00:00"
  },
  {
    "createAt": "2023-10-17 12:26:27",
    "updateAt": "2023-10-17 12:26:27",
    "id": 10,
    "email": "user3@test.com",
    "nickName": "사용자3",
    "createdAt": "2023-10-17T12:26:27.000+00:00"
  }
]
```

### 사용자 계정 정보 상세 조회

#### Request Example

```shell
curl --location 'http://localhost:8080/admin/account/16' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTMzMjc2LCJleHAiOjE2OTc1NjkyNzYsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.ys5w2OBBcScP-yWnHn9-_RDgHlvCuQU5Apkp9gK9J0YfZG2Ckueg4B-4Q5xOQTu3-hjOVtQSHgtCw-54-CU5Rg' \
--header 'language: KO'
```

#### Response Example

```json
{
  "id": 16,
  "email": "user9@test.com",
  "nickName": "사용자9",
  "thumbnail": null,
  "role": "USER",
  "status": "ACTIVE",
  "loginType": "DEFAULT"
}
```

### 관리자-사용자 게시물 리스트 조회

#### Request Example

```shell
curl --location 'http://localhost:8080/admin/account/article/1?page=0&limit=3' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTMzMjc2LCJleHAiOjE2OTc1NjkyNzYsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.ys5w2OBBcScP-yWnHn9-_RDgHlvCuQU5Apkp9gK9J0YfZG2Ckueg4B-4Q5xOQTu3-hjOVtQSHgtCw-54-CU5Rg' \
--header 'language: KO'
```

#### Response Example

```json
[
  {
    "createAt": "2023-10-17 11:26:00",
    "updateAt": "2023-10-17 11:26:00",
    "boardId": 1,
    "articleId": 6,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "미쯔야~",
    "content": "<b>사랑해 ~~~~~~~</b>",
    "createdUser": {
      "createAt": "2023-10-17 12:29:12",
      "updateAt": "2023-10-17 12:29:12",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-17 12:29:12",
      "updateAt": "2023-10-17 12:29:12",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  },
  {
    "createAt": "2023-10-17 11:25:36",
    "updateAt": "2023-10-17 11:25:36",
    "boardId": 3,
    "articleId": 5,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "엘라스틴했어요",
    "content": "<p>배고파요 흑흑흑</p>",
    "createdUser": {
      "createAt": "2023-10-17 12:29:12",
      "updateAt": "2023-10-17 12:29:12",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-17 12:29:12",
      "updateAt": "2023-10-17 12:29:12",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  },
  {
    "createAt": "2023-10-17 11:25:20",
    "updateAt": "2023-10-17 11:25:20",
    "boardId": 2,
    "articleId": 4,
    "orgArticleLanguage": "KO",
    "status": "PUBLIC",
    "likeCount": 0,
    "contentLanguage": "KO",
    "title": "오리엔탈",
    "content": "<div>유니티</div>",
    "createdUser": {
      "createAt": "2023-10-17 12:29:12",
      "updateAt": "2023-10-17 12:29:12",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "updatedUser": {
      "createAt": "2023-10-17 12:29:12",
      "updateAt": "2023-10-17 12:29:12",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    }
  }
]
```

### 관리자-사용자 댓글 조회

#### Request Example

```shell
curl --location 'http://localhost:8080/admin/account/reply/1?page=0&limit=3' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTMzMjc2LCJleHAiOjE2OTc1NjkyNzYsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.ys5w2OBBcScP-yWnHn9-_RDgHlvCuQU5Apkp9gK9J0YfZG2Ckueg4B-4Q5xOQTu3-hjOVtQSHgtCw-54-CU5Rg' \
--header 'language: KO'
```

#### Response Example

```json
[
  {
    "createAt": "2023-10-17 12:05:33",
    "updateAt": "2023-10-17 12:05:33",
    "articleId": 5,
    "replyId": 8,
    "orgReplyLanguage": "EN",
    "createUser": {
      "createAt": "2023-10-17 12:30:44",
      "updateAt": "2023-10-17 12:30:44",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "나는 매우 슬프다"
  },
  {
    "createAt": "2023-10-17 12:05:20",
    "updateAt": "2023-10-17 12:05:20",
    "articleId": 5,
    "replyId": 7,
    "orgReplyLanguage": "KO",
    "createUser": {
      "createAt": "2023-10-17 12:30:44",
      "updateAt": "2023-10-17 12:30:44",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "뿌애애애애애애앵"
  },
  {
    "createAt": "2023-10-17 12:05:00",
    "updateAt": "2023-10-17 12:05:00",
    "articleId": 5,
    "replyId": 6,
    "orgReplyLanguage": "KO",
    "createUser": {
      "createAt": "2023-10-17 12:30:44",
      "updateAt": "2023-10-17 12:30:44",
      "id": 1,
      "email": "admin@test.com",
      "password": "$2a$10$dGUjr0mYEzEl55j8J0eJdeGHtPtIYENiQj04hVrscU37bySJl4Bla",
      "nickName": "관리자",
      "thumbnail": null,
      "role": "ADMIN",
      "oauthInfo": null,
      "status": "ACTIVE",
      "loginType": "DEFAULT"
    },
    "content": "코드싸개가 흑흑흑"
  }
]
```

### 관리자계정 추가

#### Request Example

```shell
{
    "email" : "admin1234@test.com",
    "nickName" : "테스트용 관리자 추가",
    "password" : "11111112"
}
```

#### Response Example

```json

```

### 계정 상태값 변경

#### Request Example

```shell
curl --location --request PUT 'http://localhost:8080/admin/account/status' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NTMzMjc2LCJleHAiOjE2OTc1NjkyNzYsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.ys5w2OBBcScP-yWnHn9-_RDgHlvCuQU5Apkp9gK9J0YfZG2Ckueg4B-4Q5xOQTu3-hjOVtQSHgtCw-54-CU5Rg' \
--header 'language: KO' \
--header 'Content-Type: application/json' \
--data '{
    "userId" : 2,
    "status" : "DELETE"
}'
```

#### Response Example

```json

```

## File

<table>
    <thead>
        <tr>
            <th>RequestMapping</th>
            <th>Method</th>
            <th>Mapping</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=2><code>/file</code></td>
            <td><code>POST</code></td>
            <td><code>/images</code></td>
            <td>이미지 리스트 등록</td>
        </tr>
        <tr>
            <td><code>POST</code></td>
            <td><code>/image</code></td>
            <td>이미지 등록</td>
        </tr>
    </tbody>
</table>

### 이미지 리스트 등록

#### Request Example

```shell
curl --location 'http://localhost:8080/file/images' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAzNTQ1LCJleHAiOjE2OTc2Mzk1NDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.RjqfcRyI9JzgEDqOxWC9xk_ZBns9skxns3V0F9qdLpjl7mTyjl5CyWjQ5xa7MueXYgq2V3TFTiYqaBttizAIeA' \
--form 'naverObjectStorageUsageType="ARTICLE"' \
--form 'images=@"/Users/kimjongwon/Downloads/OrientalUtnityLogo Original.png"' \
--form 'images=@"/Users/kimjongwon/Downloads/HookKiller (1).png"' \
--form 'images=@"/Users/kimjongwon/Downloads/HookKiller.png"'
```

#### Response Example

```json
[
  {
    "filePath": "local/article/2023/10/18/f2d103d6-4e7e-405b-9589-3aee79dd4807.png",
    "usageType": "ARTICLE"
  },
  {
    "filePath": "local/article/2023/10/18/405c08a7-90ea-4518-abdf-4db342da8d79.png",
    "usageType": "ARTICLE"
  },
  {
    "filePath": "local/article/2023/10/18/8fff5db1-13b6-4e28-a246-e3f9f5b58ff5.png",
    "usageType": "ARTICLE"
  }
]
```

### 이미지 등록

#### Request Example

```shell
curl --location 'http://localhost:8080/file/image' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjk3NjAzNTQ1LCJleHAiOjE2OTc2Mzk1NDUsImlzcyI6Imhvb2traWxsZXIiLCJ0eXBlIjoiQUNDRVNTX1RPS0VOIiwicm9sZSI6IkFETUlOIn0.RjqfcRyI9JzgEDqOxWC9xk_ZBns9skxns3V0F9qdLpjl7mTyjl5CyWjQ5xa7MueXYgq2V3TFTiYqaBttizAIeA' \
--form 'naverObjectStorageUsageType="ARTICLE"' \
--form 'image=@"/Users/kimjongwon/Downloads/OrientalUtnityLogo Original.png"'
```

#### Response Example

```json
{
  "filePath": "local/article/2023/10/18/17fe98ec-7fca-46e4-9512-d83e868e6f76.png",
  "usageType": "ARTICLE"
}
```
