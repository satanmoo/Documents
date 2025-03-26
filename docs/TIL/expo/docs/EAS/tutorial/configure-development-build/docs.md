# Configure a development build in cloud

- [docs](https://docs.expo.dev/tutorial/eas/configure-development-build/)

## Understanding development builds

> A development build is a debug version of our project.
> It is optimized for quick iterations when creating an app.
> It contains the expo-dev-client library, which offers a robust and complete development environment.
> This setup allows us to integrate any native library or change code inside the native directories as required.

- "development build" 의 역할을 확인할 수 있음
    - native 코드의 변화를 빌드에 반영하기 위함

## Install expo-dev-client library

### Start the development server

> Let's notice the changes installing the expo-dev-client library:

> The manifest URL contains expo-development-client along with the app scheme
> The development server now operates for a development build (instead of Expo Go).

- manifest URL:
    - 말 그대로 앱의 구성 정보와 메타데이터를 담은 URL
    - 아래 URL에서 앱의 이름, "development build"를 사용하기 위해 설치한 expo-development-client 라이브러리 이름 등을 확인할 수 있음

```shell
 › Metro waiting on exp+mock-mobile-client://expo-development-client/?url=http%3A%2F%2F172.30.1.78%3A8081
```

> Since we do not have a development build installed on one of our devices or an emulator/simulator, we can't run our project yet.

- Expo Go와 마찬가지로 native app(development build)을 모바일 디바이스/에뮬레이터에 설치해야지 프로젝트를 실행할 수 있음
    - 번들러에서 JS 번들 받아서 실행
