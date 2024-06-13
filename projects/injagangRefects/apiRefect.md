# API 요청

## 기존 문제점 분석

> 기존의 API요청 로직은 하드 코딩 방식을 사용하여 API를 모듈화 하지 않았습니다. 이 방식은 여러가지 문제를 유발 하였습니다.

- URL을 잘못 입력하거나 변경될 경우, 수동으로 모든 링크를 수정해야하는 번거로움
- 비동기 함수 로직의 관심사 분리가 어려워 URL의 의도를 파악하기 어려움
- 단순히 기능 구현에 초점을 맞춘 유지보수가 어려운 상태

```jsx
export const getProfile =
  () =>
  async (dispatch: Dispatch<authDispatchType>): Promise<void> => {
    try {
      // console.log({ headers });
      const response = await fetcher(METHOD.GET, "/info", {
        headers: { Authorization: Cookies.get("accessToken") },
      });
      if (response) {
        dispatch({
          type: PROFILE_SUCCESS,
          payload: {
            nickname: response.data.nickname,
            role: response.data.role,
          },
        });
      }
    } catch (error: any) {
      dispatch({ type: AUTHENTICATE_FAILURE, payload: { error } });
    }
  };
```

## 문제점 해결방안

### Axios 인스턴스 활용

> Axios의 기능을 활용하지 못하였습니다.  
주로, JSON 문자열 처리의 간편한 목적에 사용하였습니다.

문제점 : 매 요청마다 쿠키에 인증에 필요한 정보를 직접 추출하여 입력해야 하여 코드의 가독성을 저하시키며 유지보수를 어렵게 만들었습니다.

해결법 : Axios 인스턴스를 생성하고 기본 헤더 설정 로직을 추가하여 매 요청 시 존재하는 토큰이 자동으로 헤더에 추가 될 수 있게 구현하였습니다.

```jsx
import axios from "axios";
import { SERVER } from "./config";
import Cookies from "js-cookie";
import { error } from "console";
export const API = axios.create({
  baseURL: SERVER,
  headers: {
    "Content-Type": "application/json",
  },
});

API.interceptors.request.use(
  config => {
    if (Cookies.get("accessToken")) {
      config.headers.Authorization = Cookies.get("accessToken");
    }
    return config;
  },
  error => {
    return Promise.reject(error);
  },
);

export enum METHOD {
  GET = "get",
  POST = "post",
  PUT = "put",
  PATCH = "patch",
  DELETE = "delete",
}

/**AXIOS를 좀더 오류 없이 사용하기위한 역할 */
export const fetcher = async (
  method: METHOD,
  url: string,
  ...rest: { [key: string]: any }[]
) => {
  const res = await API[method](url, ...rest);
  return res;
};
```

### 도메인별 API 엔트리 포인트

: 각 도메인별 API엔트리포인트를 객체화 하였습니다.

```jsx
export const SERVER = process.env.BACKEND_SERVER_API;

export const AUTH_APIS = {
  INFO_API: `${SERVER}/info`,
  SIGNUP_API: `${SERVER}/signup`,
  SIGNIN_API: `${SERVER}/login`,
  LOGOUT_API: `${SERVER}/logout`,
  TOKKEN_REISSUE_API: `${SERVER}/reissue`,
  NICK_CHNAGE_API: `${SERVER}/nicknameChange`,
  PASSWORD_CHAGNE_API: `${SERVER}/passwordChange`,
};
```

### 비동기 처리 함수를 모듈화

:API/도메인/도메인명API 구조로 관리하였습니다.

```jsx
export const authInfoAPI = async () => {
  return fetcher(METHOD.GET, AUTH_APIS.INFO_API);
};
```

## 결과

- 함수의 책임이 명확히 분리되어 API에 대한 책임에만 집중하게 되었습니다.
- 세부 사항은 authInfoAPI(도메인API)에 위임하여 유지보수 효율이 향상되고, 하드코딩으로 인한 오류를 방지할 수 있었습니다.

```jsx
export const getProfile =
  () =>
  async (dispatch: Dispatch<authDispatchType>): Promise<void> => {
    try {
      const response = await authInfoAPI();
      if (response) {
        dispatch({
          type: PROFILE_SUCCESS,
          payload: {
            nickname: response.data.nickname,
            role: response.data.role,
          },
        });
      }
    } catch (error: any) {
      dispatch({ type: AUTHENTICATE_FAILURE, payload: { error } });
    }
  };
```