# Develop

- [expo-cli/develop](https://docs.expo.dev/more/expo-cli/#develop)

## Bundler & Development server

- 코드를 실행 가능한 상태로 만드는 과정에서 Bundler가 어떤 역할을 하는가?
    - TypeScript로 애플리케이션 코드 작성(React Native로 앱의 UI와 로직 등을 구현)
    - 번들링
        - 표준 Javscript 코드로 변환(Transpilation)
        - 의존성 처리
        - 압축
        - ...
    - 클라이언트는 실제로 JavaScript 엔진(예: Hermes, JavaScriptCore 등)을 통해 번들된 코드를 실행
- 대략적으로 위와 같은 과정을 거친다.

- 번들러가 서버-클라이언트 구조에서 서버의 역할을 한다.
- 즉 번들러는 클라이언트가 실행할 수 있는 결과를 만들어 서빙한다.
- 여기서 `Expo-Go`, `Development Builds`(expo-dev-client 설치 필요함), 실제 모바일 디바이스 등이 클라이언트에 속한다.

- 그래서 문서에서 "development server"라는 용어를 사용하는데, 이는 "Metro Bundler"와 동일하다.
    - Expo는 default로 Metro Bundler를 사용하기 때문이다.

> Start a development server to work on your project by running:
> ```shell
> npx expo start 
> ```
> This command starts a server on http://localhost:8081 that a client can use to interact with the bundler. The default bundler is Metro.

- 이 명령어를 실행하면 개발 서버(Metro bundler)가 실행된다.
    - 즉 번들러는 http 서버
- [metrobundler/getting-started](https://metrobundler.dev/docs/getting-started)문서의 코드에서도 확인할 수 있다.

- 그래서 default 상태라면
    - Bundler host: localhost
    - Bundler port: 8081

## Launch target

- 번들러가 서버의 역할을 한다면, 여기서 target은 클라이언트의 역할

> The npx expo start command automatically launches the app in a development build if expo-dev-client is installed in the project. Otherwise, it launches the app in Expo Go.

- expo cli는 일종의 manager 역할을 수행한다.
    - 어떤 클라이언트를 실행할지 결정함
    - 이 클라이언트는 launch app 함

## Server URL

> By default, the project is served over a LAN connection. You can change this behavior to localhost-only by using the flag npx expo start --localhost.

- 보통 동일한 Wi-Fi 사용해서 작업하는 경우 개발하는 컴퓨터와 모바일 디바이스가 동일한 LAN에 속하게 됨

```shell
npx expo start
```

- 위를 실행하면 콘솔에 아래와 같은 문구를 확인할 수 있음

```text
Metro waiting on exp://172.30.1.78:8081
```

- LAN의 고유한 private ip, 포트 번호를 확인할 수 있음

- 따라서 에뮬레이터로 작업한다면 `npx expo start --localhost` 로 실행해도 무방함

> --port: Port to start the dev server on (does not apply to webpack or tunnel URLs).

- "webpack" 은 별도의 번들러임
    - "webpack" 자체의 기본 동작에 따라 포트가 결정되기 때문에 적용할 수 없음
    - 즉 expo에서 사용하는 기본 번들러(Metro Bundler)를 사용할 때 `--port` 옵션을 사용할 수 있음

### Tunneling

- Expo에는 `ngrok`이라는 프록시 서버가 내장되어 있어서 외부 디바이스로 부터 인터넷 접속을 가능하게 해줌
- 따라서 기본 번들러의 어떤 포트를 사용할지 결정하는 `--port` 옵션을 사용할 수 없음

> This will serve your app from a public URL like: http://xxxxxxx.bacon.19000.exp.direct:80.

- 터널링을 사용하는 경우 public URL을 프록시 서버에서 생성해준다.
- URL 앞에 xxxxxx는 임의의 문자열을 의미함
    - entropy

#### Drawbacks

> Tunnel URLs are public and can be accessed by any device with a network connection. Expo CLI mitigates the risk of exposure by adding entropy to the beginning of the URL. Entropy can be reset by clearing the .expo directory in your project.

- .expo 폴더를 리셋하면 entropy를 초기화할 수 있음
- 동일한 개발 세션에는 터널링한 public URL을 일관되게 유지할 수 있음

### Offline

> Offline will prevent the CLI from making network requests. If you don't use the flag and your computer has no internet connection, then offline support will automatically be enabled, it will just take a bit longer to verify the reachability.

- verify the reachability:
    - 인터넷상의 필요한 서버나 리소스(예: Expo 서버, 매니페스트 서명 서비스 등)에 접근할 수 있는지 확인하는 과정
    - [reachability](https://community.cisco.com/t5/network-management/what-is-reachability/td-p/698204)

- Expo CLI가 외부에 요청할 수 없게 만드는 플레그

- 번들러 서버를 localhost, LAN에 올리는 개념과는 독립적임

### .expo directory

- 프로젝트를 연 디바이스 정보(개발 컴퓨터)에 대한 정보가 있기 때문에 공유하면 안 됨
- Expo로 개발 서버를 시작하면 자동으로 생성하는 파일들을 포함함
  - 딱히 수정할 일 없음
