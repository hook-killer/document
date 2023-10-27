# 프론트 라우트 & Axios Client 작업 시 참고

## Axios Client

> [!NOTE]
> Client의 파라미터로 받아지는 Language는 각 컴포넌트에서 아래와 같이 불러올 수 있고 이걸 쓰려면 리스너도 추가해주어야 합니다.

```jsx
import { useTranslation } from "react-i18next";

const ~~~ = () => {
	const { t, i18n } = useTranslation();
	const language = i18n.language; // 이게 client의 파라미터로 전달될 language임

  useEffect(() => {
    axios를 쓰는 함수();
    // 리스너 등록
    i18n.on("languageChanged", axios를 쓰는 함수);
    // 컴포넌트가 언마운트될 때 리스너 제거
    return () => {
      i18n.off("languageChanged", axios를 쓰는 함수);
    };
  }, [i18n]);
}

// useEffect 안의 코드는 다들 달라져야 합니다 이건 그냥 예시로 써놓은거에요 가장 비슷한 컴포넌트에 가서 참고해서 코드 작성해보세요
```

---

### DefaultClient

> [!NOTE]
> 이 클라이언트는 토큰을 사용해야하는, 즉 로그인이 필요한 API를 쓸 때 사용할 클라이언트입니다.
> 이 클라이언트를 쓰는 컴포넌트는 App.js에 Route를 추가할 때 prop으로 토큰을 내려주어야 합니다.
> 아래는 Route에 prop으로 토큰을 주는 것의 예시와 DefaultClient의 코드입니다.

```jsx
let storageRole = getCookie("role");
const [token, setToken] = useState(isNull(storageToken) ? "" : storageToken);

<Route path="/mypage" element={<Mypage token={token} />} />;
```

```jsx
import axios from "axios";
import { getCookie } from "../utils/ReactCookie";
import { isNull } from "../utils/NullUtils";

// React Axios Timeout = 5s

// 기본적으로 사용할 Axios Client
export const jsonClient = (language, token) =>
  axios.create({
    baseURL: process.env.REACT_APP_BASE_URL,
    timeout: 5000,
    headers: {
      Authorization: "Bearer " + isNull(token) ? "" : token,
      language: language,
      "Content-Type": "application/json",
    },
  });

// 첨부파일을 발송 할 경우 사용 할 Axios Client
export const multiPartClient = (language, token) =>
  axios.create({
    baseURL: process.env.REACT_APP_BASE_URL,
    timeout: 5000,
    headers: {
      Authorization: "Bearer " + isNull(token) ? "" : token,
      language: language,
      "Content-Type": "multipart/form-data",
    },
  });
```

---

### MainCustomClient

> [!NOTE]
> 이 클라이언트는 토큰을 사용하지 않아도 되는, 즉 로그인이 필요하지 않은 API를 쓸 때 사용할 클라이언트입니다.
> 이 클라이언트를 쓰는 컴포넌트는 App.js에 Route를 추가할 때 prop으로 토큰을 내려주지 않아도 됩니다.
> 아래는 MainCustomClient의 코드입니다.

```jsx
import axios from "axios";
import { getCookie } from "../utils/ReactCookie";
import { isNull } from "../utils/NullUtils";

// React Axios Timeout = 5s

// 기본적으로 사용할 Axios Client
export const jsonClient = (language) =>
  axios.create({
    baseURL: process.env.REACT_APP_BASE_URL,
    timeout: 5000,
    headers: {
      Authorization:
        "Bearer " + isNull(getCookie("token")) ? "" : getCookie("token"),
      language: language,
      "Content-Type": "application/json",
    },
  });

// 첨부파일을 발송 할 경우 사용 할 Axios Client
export const multiPartClient = (language) =>
  axios.create({
    baseURL: process.env.REACT_APP_BASE_URL,
    timeout: 5000,
    headers: {
      Authorization:
        "Bearer " + isNull(getCookie("token")) ? "" : getCookie("token"),
      language: language,
      "Content-Type": "multipart/form-data",
    },
  });
```

---

### SearchCustomClient

> [!NOTE]
> 이건 신경안써도 되는 클라이언트입니다.
> 언어도 신경 안쓰고 토큰도 필요하지 않은 검색에서만 쓰는 클라이언트입니다.

```jsx
import axios from "axios";
import { getCookie } from "../utils/ReactCookie";
import { isNull } from "../utils/NullUtils";

// React Axios Timeout = 5s

// 기본적으로 사용할 Axios Client
export const jsonClient = axios.create({
  baseURL: process.env.REACT_APP_BASE_URL,
  timeout: 5000,
  headers: {
    Authorization:
      "Bearer " + isNull(getCookie("token")) ? "" : getCookie("token"),
    language: getCookie("language"),
    "Content-Type": "application/json",
  },
});

// 첨부파일을 발송 할 경우 사용 할 Axios Client
export const multiPartClient = axios.create({
  baseURL: process.env.REACT_APP_BASE_URL,
  timeout: 5000,
  headers: {
    Authorization:
      "Bearer " + isNull(getCookie("token")) ? "" : getCookie("token"),
    language: getCookie("language"),
    "Content-Type": "multipart/form-data",
  },
});
```
