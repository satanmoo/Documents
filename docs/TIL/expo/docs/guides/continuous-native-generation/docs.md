# Continuous Native Generation (CNG)

> A single native project on its own is complicated to maintain, scale, and update. In a cross-platform app, you have multiple native projects that you must maintain and keep up to date with the latest operating system releases to avoid falling too far behind in any third-party dependencies.

> As your native project grows, the complexity from third-party dependencies increases, complicating upgrades and slowing down developer momentum. This discourages adding advanced native functionality and leads to less powerful apps. In cross-platform apps, this complexity is multiplied across each platform.

- native project 의존성 관리하기 어려움
    - CNG에서 이를 도와준다.

> To address this, we've introduced the concept of Continuous Native Generation.
> Instead of creating native projects a single time and maintaining customizations to those native projects for the lifetime of the codebase, short-lived native projects are generated only when needed, such as when debugging or building.
> These projects are generated from a standard template plus configuration or custom code that defines how the template should be customized. The result is a native project that can be compiled into a native app with any customizations desired by the developer. However, the developer is responsible for only maintaining the definition of their customizations, rather than all of the native project code.

- 직접 native codebase를 관리하는 것이 아니라
- 네이티브 코드가 configuration, template, custom code 등을 바탕으로 on-demand(동적으로) 생성됨

## CNG in React Native apps

- [docs](https://docs.expo.dev/workflow/continuous-native-generation/#cng-in-react-native-apps)

> React Native apps can use CNG by using Prebuild to automate upgrades, install or uninstall libraries, apply white label customizations, share configuration across multiple apps, reduce orphaned code, and more.

- `prebuild` 개념은 CNG를 사용하는 개념
    - Expo에서 이 명령어로 CNG 할 수 있음
