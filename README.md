## 인증

laravel 11 breeze에서는 sanctum이 기본적으로 적용되어있다.
router:list를 확인하면 아래와 같이 auth관련 endpoint가 추가되어있는 것을 확인할 수있다.

```
  GET|HEAD   / .......................................................................................... 
  GET|HEAD   api/user ................................................................................... 
  POST       email/verification-notification verification.send › Auth\EmailVerificationNotificationContr…
  POST       forgot-password .................... password.email › Auth\PasswordResetLinkController@store
  POST       login .................................... login › Auth\AuthenticatedSessionController@store
  POST       logout ................................ logout › Auth\AuthenticatedSessionController@destroy
  POST       register .................................... register › Auth\RegisteredUserController@store
  POST       reset-password ........................... password.store › Auth\NewPasswordController@store
  GET|HEAD   sanctum/csrf-cookie ...... sanctum.csrf-cookie › Laravel\Sanctum › CsrfCookieController@show
  GET|HEAD   up ......................................................................................... 
  GET|HEAD   verify-email/{id}/{hash} .................. verification.verify › Auth\VerifyEmailController

                                                                                      Showing [11] routes
```

## vscode Thunder Client

vscode의 Thunder Client를 이용해서 api 테스트를 할 것이다. Thunder Client 또는 postman 모두 동일하다.

laravel 11에서는 [csrf](https://laravel.com/docs/11.x/csrf)(교차 사이트 요청 위조)라는 사이트간 요청 위조를 방지하기위한 방법이 들어가있다.

api에서 이것을 어떻게 해결할 것인가?

1. Env를 추가
2. Env에 값으로 XSRF-TOKEN 추가
3. `GET /sanctum/csrf-cookie` 요청 생성
4. `GET /sanctum/csrf-cookie` 요청에 Tests 탭에 XSRF-TOKEN env에 저장하는 로직 추가
    ```js
    const _ = await tc.loadModule('lodash');

    const res = tc.response

    // 쿠키에서 xsrf-token을 가져온다
    const xsrfToken = _.find(res.cookies, { name: 'xsrf-token' }).value;

    // fullsatck-todo-laravel-react env에 해당값 저장
    tc.setVar("XSRF-TOKEN", xsrfToken)
    ```
5. 이제 실제 요청 API에서 header에 `X-XSRF-TOKEN: {{XSRF-TOKEN | urlDecode}}`추가해서 요청

## 참고

- https://github.com/Hansanghyeon/laravel-starter
- https://www.toptal.com/laravel/restful-laravel-api-tutorial
