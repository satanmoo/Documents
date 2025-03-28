# Introduction to Expo Router

> It brings the best file-system routing concepts from the web to a universal application — allowing your routing to work across every platform. When a file is added to the app directory, the file automatically becomes a route in your navigation.

- 파일 시스템 개념으로 동작
- `app` 폴더에 파일을 넣어서 앱의 경로 처럼 만들 수 있음

## Features

> Shareable: Every screen in your app is automatically deep linkable. Making any route in your app shareable with links.

- [딥 링크](https://docs.tosspayments.com/resources/glossary/deep-link)

> Discoverable: Expo Router enables build-time static rendering on web, and universal linking to native.
> Meaning your app content can be indexed by search engines.

- 정적인 파일로 랜더링될 수 있으면 검색 엔진에서 인덱싱이 쉬움

## Common questions

### what-are-the-benefits-of-file-based-routing

> Expo Router has the ability to statically type routes automatically.
> This ensures you can only link to valid routes and that you can't link to a route that doesn't exist.
> Typed Routes also improve refactoring as you'll get type errors if links are broken.

- 컴파일 타임에 라우터 경로 검증

> Async Routes (bundle splitting) improve development speed, especially in larger projects.
> They also make upgrades easier as errors are isolated to a single route, meaning you can incrementally update or refactor your app page-by-page rather than all at once (traditional React Native).

- 각 라우트를 필요할 때 비동기로 불러오는 방식
    - 실험 중인 기능 같음
- DOM 일부만 다시 랜더링하는 개념과 유사함

