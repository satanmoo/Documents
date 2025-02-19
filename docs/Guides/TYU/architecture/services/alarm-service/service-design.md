# Alarm Service Design

## Objective

- 유저가 지정한 시간에 자주 방문하는 위치의 날씨를 조회하고 우산이 필요하면 푸시 알림
- 유저는 알림을 다른 사람에게 공유하고, 앱 설치를 유도

## Key Features

- 유저 조회:
    - 알림을 받을 시간에 설정된 유저를 조회
- 날씨 조회:
    - 유저가 자주 방문하는 위치의 날씨를 조회
- 알림 전송:
    - 날씨가 우산이 필요한 조건이면 푸시 알림 전송
- 알림 공유:
    - 사용자가 알림을 다른 사람과 공유
- 앱 설치 유도:
    - 앱 설치하지 않은 사용자에게는 웹 링크를 통해 앱 설치를 유도

## Data Flow

1. 사용자 일정 조회:
    - 서비스는 알림 시간이 설정된 유저들을 사용자 데이터베이스에서 조회합니다.

2. 날씨 조회:
    - 각 유저에 대해 자주 방문하는 위치의 날씨 데이터를 조회합니다.
    - 날씨가 우산이 필요할 수 있는 조건인지를 확인합니다 (예: 비, 눈 등).

3. 알림 생성:
    - 날씨가 우산이 필요한 조건이라면, 알림 메시지를 준비합니다.

4. 푸시 알림 전송:
    - 서비스는 설정된 시간에 맞춰 유저에게 푸시 알림을 전송합니다.

5. 알림 공유:
    - 사용자는 알림을 웹 링크나 푸시 알림으로 다른 사람과 공유할 수 있습니다.
    - 알림을 받은 사람이 앱을 설치하지 않았다면, 웹 링크를 통해 앱 다운로드 페이지로 유도합니다.
