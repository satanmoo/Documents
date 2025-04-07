# Ios Deploy

## Test Flight

### 빌드하기

- eas.json

```text
"production": {
      "env": {
        "ENV": "production"
      },
      "ios": {
        "simulator": false
      }
    }
```

- eas.json 빌드 프로필에 ios 추가하기
    - simulator:
        - false
        - 실제 디바이스에서 실행 가능한 빌드를 생성
    - [ref](https://docs.expo.dev/eas/json/#simulator)

```text
"submit": {
    "production": {
      "ios": {
        "appleId": "",
        "ascAppId": "",
        "ascApiKeyPath": "",
        "ascApiKeyIssuerId": ""
      }
    }
  }
```

- 옵션 채우기
    - Apple App Store Connect API
    - Apple Developer

```shell
eas build --platform ios --profile production
```

- 빌드 완료

### App store connect에 앱 래코드 만들기

- [App store connect](https://appstoreconnect.apple.com/)
- 

### Test Fight 제출

```shell
eas submit --platform ios --profile production --latest
```

- 방금 빌드 결과 App Store Connect 제출
- cli 가이드 잘 따라가면 됨;

### 테스트자 초대

- 외부 테스터 그룹으로 초대
- 외부 테스터에 빌드 추가하려면 테스트 정보를 입력해야함
  - 베타 앱 정보
    - 전화번호:
      - +8210xxxxxxxx
      - 이 포맷으로 등록