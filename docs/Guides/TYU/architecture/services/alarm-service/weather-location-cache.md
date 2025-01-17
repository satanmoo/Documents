# Weather Location Cache

## Objective

- 위치 - 날씨 매핑 데이터를 저장해 알림 서비스를 위한 빠른 조회를 가능하게 함

## Cache Data Structure

- Key Format:
    - weather:{ny}:{nx}:{base_time}

- Value Format:
    - 저장할 값은 json 객체로 구성하여, 날씨 예보 시간마다 after_1_hour부터 after_6_hour까지의 강수 예보 값을 포함

```json
{
  "after_1_hour": 1,
  "after_2_hour": 0,
  "after_3_hour": 0,
  "after_4_hour": 1,
  "after_5_hour": 0,
  "after_6_hour": 1
}
```

## Data Flow

1. DB에서 캐시로 데이터 로딩
2. 알림 전송 시 캐시 조회
3. 캐시 갱신
    - 날씨 데이터 새로 갱신 되면 업데이트
4. 캐시 만료
    - 기준 시간(base_time) + 1시간
