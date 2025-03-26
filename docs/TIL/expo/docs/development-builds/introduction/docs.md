# Introduction to development builds

- [docs](https://docs.expo.dev/develop/development-builds/introduction/)

> The native app (Expo Go) is immutable once installed. Native build tools are required to create this bundle, and it needs to be signed to be installable on real devices. To add a new library with native code or change metadata that is shipped with the app (for example app name, icon, splash screen) the app needs to be rebuilt and re-installed on the device.

- "native app":
    - 특정 플랫폼(e.g. iOS, Android)에서 직접 실행되도록 설계된 어플리케이션
- 위 문장은 "native app"이 immutable함을 설명한다.
    - native code가 변하면 새로운 빌드가 필요함
    - 기존의 "native app"을 변경할 수 없기 때문
- "Native build tool":
    - iOS의 Xcode, Android의 Gradle 등 네이티브 바이너리를 생성하기 위한 도구들

> The JavaScript bundle (npx expo start) is where your app's UI code and business logic are. In production apps, there is one main.js bundle that is shipped with the app itself.
> In development, this JS bundle is live reloaded from your local machine.
> The main role of React Native is to provide a way for the JavaScript code to access the native APIs (Image, Camera, Notifications, and more). However, only APIs and libraries that were bundled in the native app can be used.

- "npx expo start" expo cli에서 실행하면 Metro bundler에서 번들을 생성해서 제공함
- production app(실제 사용자에게 배포되는 앱)은 한 번 생성된 main.js라는 번들이 배포된다.
    - 앱이 설치 될 때 같이 설치됨
    - immutable
- 개발 환경에서는 자바 스크립트 코드를 변경하면 Metro bundler가 번들을 생성하고 live reloading
    - mutable
- React Native의 주요 역할은 번들에 포함된 native APIs에 대한 접근을 제공하는 것
    - 당연히 번들에 포함된 native code(APIs)만 접근할 수 있다.

> Since the native app bundle is immutable, this means that when you're using the Expo Go app for development, you can only rely on the native code and tools that exist in Expo Go.
> The only way to get around it is to build your native app yourself instead of using Expo's pre-packaged sandbox.
> This is exactly what a Development Build is, your own version of Expo Go, where you are free to use any native libraries and change any native config.

- Expo go는 기본적으로 미리 포함된 네이티브 코드를 가지고 있음
    - pre-packaged sandbox
    - 그리고 위 문단에서 설명한대로 native code는 immutable하기에 사용 중에 수정할 수 없음
    - 따라서 미리 포한된 native 기능만 사용할 수 있음
- Expo go에 추가적인 native 코드(라이브러리)를 추가하면 직접 native 앱을 빌드하게 됨
    - 이 빌드가 "Development Build"
- Expo go의 커스텀 버젼이 "Development Build" 라고 생각하면 됨

## Why use a development build (a.k.a what can't you do in Expo Go and why)

- [docs](https://docs.expo.dev/develop/development-builds/introduction/#why-use-a-development-build-aka-what-cant-you-do-in-expo-go-and-why)

### Use libraries with native code that aren't in Expo Go

> Consider react-native-webview as an example, a library that contains native code, but is included in Expo Go.
> When you run npx expo install react-native-webview command in your project, it will install the library in your node_modules directory, which includes both the JS code and the native code.
> But the JS bundle you are building only uses the JS code. Then, your JS bundle gets uploaded to Expo Go, and it interacts with the native code that was already bundled with the app.

- 중요한 개념이 포함된 문단이다.
- "react-native-webview"는 네이티브 코드를 포함하고, Expo Go에 내장되어있음
    - 즉 Expo Go에 "react-native-webview"의 네이티브 코드가 포함되어있음
- 개발 시 "npx expo install react-native-webview"를 실행하면 개발 로컬에 모듈을 설치하게 된다.
- 하지만 JS bundler은 이 모듈에서 오직 JS code만 사용함
- JS bundler가 네이티브 코드를 사용하지 못해도 괜찮은 이유는 번들이 Expo Go에 업로드된 후 Expo Go에 내장된 "react-native-webview"의 네이티브 코드와 상호작용 할 수 있기 때문

#### 왜 `npx expo install react-native-webview` JS 번들에서 네이티브 코드는 사용하지 않는데 로컬에 다운로드 받는 걸까?

- 소스 코드를 보면 라이브러리에 네이티브 코드가 포함되어 있음
    - 그래서 다운 받으면 같이 받아지긴 함
- 근데 로컬에서 어떻게 사용되나?
    - 네이티브 API에 대한 인터페이스 정의
    - TypeScript에서 타입 검사
    - Expo Go를 사용하지 않고 로컬에서 빌드를하면 필요하겠죠

> Instead, when you try to use a library that is not included, for example, react-native-firebase, then you can use the JS code and hot reload the new bundle into Expo Go but it will immediately error because the JS code tries to call the native code from the React Native Firebase package that does not exist in Expo Go.
> There is no way to get the native code into the Expo Go app unless it was already included in the bundle that was uploaded to the app stores.

- 반면 네이티브 코드가 Expo Go에 내장되지 않은 라이브러리의 경우 실행 할 수 없음
- 그래서 "Development Build"가 필요함
