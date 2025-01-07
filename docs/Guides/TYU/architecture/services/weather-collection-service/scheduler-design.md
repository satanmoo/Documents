# Scheduler Design

## Objective

- 초단기예보 API 호출을 자동화해 데이터 수집 주기를 유지

## Requirements

- 주기적 실행:
    - 매시 정각에 API 호출
        - 1:1 문의 답변 결과보고 결정
        - TODO
- 신뢰성:
    - 호출 실패 시 재시도 메커니즘

## Tool

- Spring Scheduler
- Spring Retry

## Error Handling

- 알림
    - 실패, 재시도 상황 발생 시 이메일 보내기
