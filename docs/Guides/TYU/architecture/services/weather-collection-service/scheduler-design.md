# Scheduler Design

## Objective

- 초단기예보 API 호출을 자동화해 데이터 수집 주기를 유지

## Approach

- 매 10분마다 초단기 예보 격자자료 API 호출
    - 기준 시간 기준 6시간 이후까지 조회
- 각 격자에 대한 강수형태(PTY) 코드를 데이터베이스에 저장
