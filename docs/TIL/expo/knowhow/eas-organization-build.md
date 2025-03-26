# How to eas build by expo organization

- `app.json` 파일에서 `eas init` 명령어 관련 필드가 존재하면 제거해야함
    - expo.extra.eas.projectId
    - expo.owner
- 기존에 프로젝트가 설정이 존재하면 "To re-configure, remove the "extra.eas.projectId" field from your app config." 과 같은 메시지가 콘솔에 나옴
- 관련 필드를 다 제거하고 `eas init` 실행하고 콘솔 절차 밟으면 됩니다.

- [reference](https://stackoverflow.com/questions/75611436/how-do-i-eas-build-into-an-expo-organization-instead-of-my-personal-expo-account)
