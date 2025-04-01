# Use a development build

> Usually, creating a new native build from scratch takes long enough that you'll be tempted to switch tasks and lose your focus.
> However, with the development build installed on your device or an emulator/simulator, you won't have to wait for the native build process until you change the underlying native code that powers your app.

- JS 코드는 동적으로 번들러에서 업데이트
- 네이티브 관련 코드를 수정하면 다시 빌드가 필요함
    - power: 앱을 구성한다는 의미
- 이 문단에서 말하려는 것은 `development build` 가 왜 필요한가
    - 익숙한 이야기

## Add error handling

> Import expo-dev-client at the top of the App.{js|tsx} or app/_layout.tsx to add additional context for certain errors beyond what is provided by default in React Native.
> In particular, expo-dev-client will help detect situations related to a mismatch between your JavaScript and native code, such as when a native module is missing and you should make a new development build.

- additional context:
    - 기본 React Native 에러 메시지에 추가로 디버깅에 도움되는 부가적인 정보
- 특히 `expo-dev-client` 는 JS와 native code 매칭을 탐지
    - 그래서 개발자에게 새로 development build 해야하는 상황을 알려줌

> This will only affect the application in which you make this change. If you want to load multiple projects from a single development app, you'll need to add this import statement to each project.

- app/_layout.tsx 파일은 프로젝트 단위로 동작
