# Mock app

// TODO: https://docs.expo.dev/get-started/create-a-project/ template 옵션 넣어서

- 위치 수집 테스트를 위함

## Prerequisites

- [prerequisites](https://docs.expo.dev/tutorial/create-your-first-app/#prerequisites)

### Expo go

- 테스트할 디바이스에 설치

### Node.js LTS 설치

```shell
# nvm 다운로드 및 설치:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
# Node.js 다운로드 및 설치:
nvm install 22
# Node.js 버전 확인:
node -v # "v22.14.0"가 출력되어야 합니다.
nvm current # "v22.14.0"가 출력되어야 합니다.
npm 버전 확인:
npm -v # 10.9.2가 출력되어야 합니다.
```

- Version:
    - Node.js: 22.14.0(LTS)
    - nvm: 0.40.2

## Initialize a new Expo app

```shell
npx create-expo-app@latest [프로젝트 명] && cd [프로젝트 명]
```

- 프로젝트 이름과 동일한 디렉토리가 생성됨

## Run the app on mobile and web

```shell
npx expo start
```

- Install expo-dev-client
    - Developement Build 사용하기

- https://docs.expo.dev/develop/development-builds/create-a-build/#install-expo-dev-client
    - 여기 문서에서 어떤 과정 따했는지 인용만 해주면 될 듯

- https://docs.expo.dev/tutorial/eas/introduction/
  - 그리고 EAS 튜토리얼도 진행

- https://docs.expo.dev/develop/tools/
  - expo cli
  - eas cli
  - expo doctor
    - 용도는 정확하게 알아야함..
  - orbit
    - 이건 에뮬레이터
    - 