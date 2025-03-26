# Key concepts

## Continuous Native Generation (CNG)

> Continuous Native Generation (CNG) is a process for building an Expo app where your native projects are generated on-demand from your app.json and package.json, similar to how your node_modules are generated from your package.json.

- 여기서 native project는 정적인 코드로 프로젝트 리포지토리에 포함되지 않는다는 것을 알 수 있다.
- 대신에 `node_mudles` 폴더 처럼 앱을 빌드하거나 실행할 때 동적으로 native code가 생성된다.
    - 이 과정이 "CNG"의 정의

> You can add the native project directories (android and ios) to your .gitignore and/or delete the project at any time, then re-generate them from the Expo app config with npx expo prebuild whenever required. You might never even run prebuild on your own development machine if you use a cloud-based development workflow.

- 그래서 native project directory는 .gitignore에 추가해도 된다.
- configuration 파일만 있으면 언제든 재생성할 수 있다.

- [docs](https://docs.expo.dev/workflow/overview/#continuous-native-generation-cng)
