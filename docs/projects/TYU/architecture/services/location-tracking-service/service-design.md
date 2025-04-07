# Location Tracking Service Design

## Objective

- 위치 추적 서비스는 주기적으로 유저의 위치 데이터를 수집해 자주 방문하는 위치를 식별

## Service Overview

- 주요 기능:
    - 이동 거리 기반 위치 수집
    - 자주 방문하는 위치 도출
    - 유저 정보 업데이트

## Data Flow

- 데이터 수집:
    - 모바일 앱에서 위치 데이터를 수집하여 API 서버로 전송
- 데이터 변환:
    - 위,경도를 "Lambert Conformal Conic Projection"을 통해 격자쌍으로 변환
    - 격자쌍을 "Lambert Conformal Conic Projection" 역으로 변환해 위,경도를 구해 reverse geocoding 테이블 조회해 격자에 대응하는 주소를 얻음
- 데이터 저장
