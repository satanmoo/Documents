# Core concepts of file-based Routing

## The rules of Expo Router

### 1. All screens/pages are files inside of app directory

> All navigation routes in your app are defined by the files and sub-directories inside the app directory. 
> Every file inside the app directory has a default export that defines a distinct page in your app (except for the special _layout files).

- 기본적으로 하나의 파일에 하나의 컴포넌트만 정의하기 때문에 "export default"로 단일 페이지를 명확하게 지정

> Accordingly, directories inside app define groups of related screens together as stacks, tabs, or in other arrangements.

- stack, tab으로 연관 페이지를 묶는 개념

### 3. First index.tsx is the initial route

> With Expo Router, you do not define an initial route or first screen in code. 
> Rather, when you open your app, Expo Router will look for the first index.tsx file matching the / URL. 
> This could be an app/index.tsx file, but it doesn't have to be. 
> If the user should start by default in a deeper part of your navigation tree, you can use a route group (a directory where the name is surrounded in parenthesis), 
> and that will not count as part of the URL. 
> If you want your first screen to be a group of tabs, you might put all of the tab pages inside the app/(tabs) directory and define the default tab as index.tsx. 
> With this arrangement, the / URL will take the user directly to app/(tabs)/index.tsx file.

- 기본적으로 app 폴더를 기준으로 처음으로 만나는 최상위 index.tsx 파일이 첫번째 화면
  - 랜딩 페이지로 사용
- tab같은 "route group"으로 묶으면 URL의 일부분으로 취급하지 않아서 최상위 경로에 있는 것으로 간주

### 4. Root _layout.tsx replaces App.jsx/tsx

> Every project should have a _layout.tsx file directly inside the app directory. 
> This file is rendered before any other route in your app and is where you would put the initialization code that may have previously gone inside an App.jsx file, such as loading fonts or interacting with the splash screen.

- `_layout.tsx` 파일은 다른 라우트 파일보다 먼저 랜더링 됨
  - 진입점
  - 앱 전체에 영향을 주는 초기 레이아웃
- 예전에는 `App.tsx` 파일이 진입점, 이제 `_layout.tsx` 파일이 대체함

### 5. Non-navigation components live outside of app directory

> In Expo Router, the app directory is exclusively for defining your app's routes. 
> Other parts of your app, like components, hooks, utilities, and so on, should be placed in other top-level directories. 
> If you put a non-route inside of the app directory, Expo Router will attempt to treat it like a route.

- 여기서 top-level directory는 루트 바로 하위 깊이
- app 폴더랑 동일한 레이어

>  Alternatively, you can create a top-level src directory and put your routes inside the src/app directory, with non-route components going in folders like src/components, src/utils, and so on. 
> This is the only other directory structure that Expo Router will recognize.

- top-level dircetory에 src 폴더 만들고 구성해도 됨
  - 근데 추가적인 조치가 필요함

### 6. It's still React Navigation under the hood

> While this may sound quite a bit different from React Navigation, Expo Router is actually built on top of React Navigation. 
> You can think of Expo Router as an Expo CLI optimization that translates your file structure into React Navigation components that you previously defined in your own code.

- Expo Router 내부에는 React Navigation가 동작함
