# Config plugins: Introduction

> An automatic setup for adding a native module to your project is possible.
> Sometimes, a module requires a more complex setup.
> A config plugin can be used to automatically configure your native project for a module and reduce the complexity by avoiding interaction with the native project.

- config plugin의 목적:
    - native 모듈을 프로젝트에 구성하기 위함

## What is a config plugin

> Config plugin is a system for extending the app config and customizing the prebuild process for your app.
> They can be used to add native modules that aren't included, by default, or to add any native code that needs to be configured further.

- `prebuild` 개념은 네이티브 관련 기능을 빌드
- They는 Config plugin
    - 앞에서는 추상 명사로 사용했음
- Config plugin의 용도는 네이티브 코드를 빌드에 포함하는 것

> Internally Expo CLI uses config plugins to generate and configure all the native code for a managed project. Plugins do things such as generate app icons, set the app name, and configure the AndroidManifest.xml, Info.plist, and so on.

- Expo CLI 가 내부적으로 config plugin 사용
- 부연 설명

> You can think of plugins like a bundler for native projects, and running npx expo prebuild as a way to bundle the projects by evaluating all the project plugins.
> Doing so will generate android and ios directories. These directories can be modified manually after being generated, but then they can no longer be safely regenerated without potentially overwriting manual modifications.

- 부연 설명
- config plugin이 네이티브 코드 번들러의 역할
- JS 번들러와 다르게 immutable 하다는 설명

## Use a config plugin

> Expo config plugins mostly come from Node.js modules. You can install them just like other packages in your project.

- 노드의 모듈 시스템을 사용함
  - 노드에서 사용되는 패키지 매니저로 설치하고 관리할 수 있음

- 아래 부연 설명에서 `mod` 개념은 플러그인 필요하면 다음 문서 찾기
