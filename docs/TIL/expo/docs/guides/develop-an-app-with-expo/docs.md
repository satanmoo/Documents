# Develop an app with Expo

- [docs](https://docs.expo.dev/workflow/overview/)

## Key concepts

### Continuous Native Generation (CNG)

- [docs](https://docs.expo.dev/workflow/overview/#continuous-native-generation-cng)

> Continuous Native Generation (CNG) is a process for building an Expo app where your native projects are generated on-demand from your app.json and package.json, similar to how your node_modules are generated from your package.json.

- 여기서 native project는 정적인 코드로 프로젝트 리포지토리에 포함되지 않는다는 것을 알 수 있다.
- 대신에 `node_mudles` 폴더 처럼 앱을 빌드하거나 실행할 때 동적으로 native code가 생성된다.
    - 이 과정을 해주는게 "CNG"의 역할

> You can add the native project directories (android and ios) to your .gitignore and/or delete the project at any time, then re-generate them from the Expo app config with npx expo prebuild whenever required. You might never even run prebuild on your own development machine if you use a cloud-based development workflow.

- 그래서 native project directory는 .gitignore에 추가해도 된다.
- configuration 파일만 있으면 언제든 재생성할 수 있다.

## The core development loop

- [docs](https://docs.expo.dev/workflow/overview/#the-core-development-loop)

### Write and run JavaScript code

> This involves creating components, writing business logic, or installing libraries from npm that don't require native code changes.
> The changes you make here are reflected in your app without needing any interaction with the native side of your app.

- 네이티브와 관련 없는 기능 개발
- Bundler의 hot-reloading 가능
- Js 코드가 추가되더라도 새로 빌드할 필요 없음

### Update app configuration

> This involves modifying your app's configuration using the app config file (app.json or app.config.js). It includes updating your app's name, icon, splash screen, and other properties. These changes don't all affect the native project directly.
> However, if you make changes that affect the native projects, you can use the app config to modify the native project configuration or create or use a config plugin. See app config reference for a complete list of properties available in the app config file.

- app config 확장해서 네티이브 관련 설정을 바꾸면 다시 빌드해야함
    - config plugin

### Write native code or modify native project configuration

> This includes writing native code directly or modifying native code configuration.
> You either need access to the native code project directories to make these changes, or you can write native code with a local Expo Module.

- 당연히 네티이브 코드 직접 수정하면 다시 빌드해야함

### Install a library that requires native code modifications

> This includes that a library requires making changes to the native code project configuration. Either the library provides a config plugin or steps to take to update the app config. Like the previous activity, this also requires you to create a development build.

- 위와 동일

> When creating a development build, you have two options. You can create a cloud-based build using EAS Build or do it locally.
> If you choose to do it locally, you can use CNG and then npx expo prebuild --clean, or you can create a development build using npx expo run android|ios or Android Studio and Xcode.

- `prebuild` 명령어로 CNG를 사용하는 상황은 로컬에서 빌드할 때
- EAS 사용하는게 여로모로 편함

> Note: When creating a development build locally, the npx expo run commands will generate native directories before building your app.
> If you modify your project's configuration or native code after the first build, you will have to rebuild your project. Running npx expo prebuild again layers the changes on the top of existing files. It may also produce different results after the build. To avoid this, add native directories to the project's .gitignore and use npx expo prebuild --clean command.

- 핵심 내용:
    - 네이티브 코드 빌드랑 JS 번들링 작업 개념을 구분해야함
        - 동적으로 JS 코드는 번들링해서 핫 리로딩 가능
        - 네이티브 코드는 다시 빌드해야함

> During your app's development loop can also install different variants (development, preview or production) of your app on the same device.

> Another key part of the development loop is debugging. See Debugging runtime issues for more information about debugging your app and learn about different debugging tools available.

- 내용 없음
