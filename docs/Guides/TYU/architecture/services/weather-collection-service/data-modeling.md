# Data modeling

## Solution 1

- 모든 시간대의 날씨를 한 번에 업데이트하는 방법 (기준시간을 기준으로 0~1h, 1~2h, … 5~6h 저장)
- ny, nx (격자 좌표)를 primary key로 사용하고, 날씨 변수 값을 0~1h, 1~2h 등으로 시간 구간을 나눠서 저장합니다.

```sql
CREATE TABLE weather_forecast
(
    ny            INT  NOT NULL,
    nx            INT  NOT NULL,
    forecast_time TIME NOT NULL, -- 기준시간
    after_1_hour  INT,           -- 예보시간 1 (기준시간+1시간)
    after_2_hour  INT,           -- 예보시간 2 (기준시간+2시간)
    after_3_hour  INT,           -- 예보시간 3 (기준시간+3시간)
    after_4_hour  INT,           -- 예보시간 4 (기준시간+4시간)
    after_5_hour  INT,           -- 예보시간 5 (기준시간+5시간)
    after_6_hour  INT,           -- 예보시간 6 (기준시간+6시간)
    PRIMARY KEY (ny, nx, forecast_time)
);
```

- 장점:
    - 한 번의 업데이트로 모든 예보 정보 저장
        - 최소한의 I/O 작업
    - 하나의 행을 조회해서 모든 예보를 확인할 수 있음

- 단점:
    - after_hour 기준이 변경될 수 있음
    - 데이터 조회 시 특정 예보시간에 대한 필터링 필요

## Solution 2

- 예보시간별로 Row로 저장하는 방식

```sql
CREATE TABLE weather_forecast
(
    ny            INT  NOT NULL, -- 격자 위도 좌표
    nx            INT  NOT NULL, -- 격자 경도 좌표
    forecast_time TIME NOT NULL, -- 기준시간 (예: 16:00, 17:00 등)
    forecast_hour INT  NOT NULL, -- 예보시간 (1, 2, 3, 4, 5, 6 등)
    pty           INT,           -- 강수 코드 (PTY) 값
    PRIMARY KEY (ny, nx, forecast_time, forecast_hour)
);
```

- 장점:
    - 유연한 예보 처리
        - 예보 시간이 늘어나거나 줄어들어도 유연하게 대응

- 단점:
    - 데이터 중복
        - ny, nx, forecast_time 값은 중복됨
    - 동시에 여러 예보 업데이트 처리 필요

## Result

- Solution 1
- 예보 시간 별로 불연속적인 데이터를 조회할 필요가 없음
- 예보 시간이 고정되어 있음
- 단건 조회에 최적화된 구조로 I/O 최소화하고 메모리나 캐시에서 처리
