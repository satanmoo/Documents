# Weather Data Source

## Contents

1. [초단기실황조회 API](#초단기실황조회-api)
2. [초단기예보조회 API](#초단기예보조회-api)
3. [단기예보조회 API](#단기예보조회-api)
4. [위.경도 격자 변환 API](#임의의-위경도를-격자로-번호로-변환하는-api)
5. [초단기 예보 격자자료 API](#초단기-예보-격자자료-api)
6. [변경 이력](#change-history)

## 초단기실황조회 API

- 설명:
    - 발표일자, 발표시, 예보지점(행정구역코드)의 좌표를 조건으로 예보지점의 실황 조회
    - Nowcasting
    - 아주 최근의 기상 상태를 관측한 데이터
    - 현재 날씨를 정확히 알기 위한 정보

- 입력:
    - 발표일자(base_date):
        - 샘플데이터:
            - 20250102
    - 발표시각(base_time):
        - 샘플데이터:
            - 0600
        - 설명:
            - 06시 발표
        - 제한:
            - 정시 단위로 입력
                - Ex) 0900
            - 매 시각 10분 이후 호출할 수 있음
                - 06 발표 데이터를 얻으러면 06시 10분 이후 호출 가능
    - 예보 지점(nx, ny):
        - 행정동 기준 지역 지점 좌표(X, Y)
        - 위도와 경도값을 좌표로 변환하는 과정이 추가적으로 필요
            - [Lambert Conformal Conic Projection](https://en.wikipedia.org/wiki/Lambert_conformal_conic_projection)
                - 변수:
                    - 사용자 위치의 위도(latitude) 경도(longitude)
                - 상수:
                    - 중앙 경도(central meridian):
                        - 참고로 본초자오선
                    - 기준 위도(latitude at which y coordinate is to be 0 on central meridian): 38.0
                    - 지도에서 사용하는 지구의 반경(radius on Earth): 6371.00877(KM)
                    - 격자 간격: 5.0(KM)
                    - first 표준 위도(first standard parallel): 30.0
                    - second 표준 위도(second standard parallel): 60.0

                * 괄호는 위키피디아에 대응되는 용어
        - API 호출해서 구할 수 있음

- 출력:
    - 자료구분코드(category):
        - RN1(1시간 강수량)
        - PTY(강수형태)
    - 실황값(obsrValue):
        - RN1:
            - x(mm)
            - 강수량 mm 단위
        - PTY:
            - 코드에 대응
            - 없음(0), 비(1), 비/눈(2), 눈(3), 빗방울(5), 빗방울눈날림(6), 눈날림(7)

- 초단기실황 데이터 생성 및 업데이트:
    - 매시간 정시에 생성되고 10분마다 최신 정보로 업데이트

## 초단기예보조회 API

- 설명:
    - 발표일자, 발표시, 예보지점(행정구역코드)의 좌표를 조건으로 예보지점의 예보 조회
    - Vert Short-term Forecast
    - 아주 최근의 기상 상태를 예측한 데이터
    - 짧은 시간 이내의 기상 변화를 예측

- 입력:
    - 발표일자(base_date):
        - 샘플데이터:
            - 20250102
    - 발표시각(base_time):
        - 샘플데이터:
            - 0630
        - 설명:
            - 06시 30분 발표
        - 제한:
            - 매 시각 30분 단위로 입력
                - Ex) 0930
            - 매 시각 45분 이후 호출할 수 있음
                - 0930 발표 데이터를 얻으러면 10시 15분 이후 호출 가능
    - 예보 지점(nx, ny):
        - 행정동 기준 지역 지점 좌표(X, Y)
        - 위도와 경도값을 좌표로 변환하는 과정이 추가적으로 필요
            - [Lambert Conformal Conic Projection](https://en.wikipedia.org/wiki/Lambert_conformal_conic_projection)
                - 변수:
                    - 사용자 위치의 위도(latitude) 경도(longitude)
                - 상수:
                    - 중앙 경도(central meridian):
                        - 참고로 본초자오선
                    - 기준 위도(latitude at which y coordinate is to be 0 on central meridian): 38.0
                    - 지도에서 사용하는 지구의 반경(radius on Earth): 6371.00877(KM)
                    - 격자 간격: 5.0(KM)
                    - first 표준 위도(first standard parallel): 30.0
                    - second 표준 위도(second standard parallel): 60.0

                * 괄호는 위키피디아에 대응되는 용어
        - API 호출해서 구할 수 있음

- 출력:
    - 자료구분코드(category):
        - RN1(1시간 강수량)
        - PTY(강수형태)
    - 예측값(fcstValue):
        - RN1:
            - x(mm)
            - 강수량 mm 단위
        - PTY:
            - 코드에 대응
            - 없음(0), 비(1), 비/눈(2), 눈(3), 빗방울(5), 빗방울눈날림(6), 눈날림(7)
    - 예측시간(fcstTime):
        - 1530 기준으로, 1600, 1700, 1800, 1900, 2000, 2100
            - 6시간 이후까지 예측
    - 예측일자(fcstDate)
        - 발표일자와 동일함

- 초단기예보 생성 및 업데이트:
    - 매시간 30분에 생성 10분마다 업데이트

## 단기예보조회 API

- 설명:
    - 발표일자, 발표시, 예보지점(행정구역코드)의 좌표를 조건으로 예보지점의 단기 예보 조회
    - Short-term Forecast
    - 5일 이내 예보 조회

- 입력:
    - 발표일자(base_date):
        - 샘플데이터:
            - 20250102
    - 발표시각(base_time):
        - 샘플데이터:
            - 0500
        - 설명:
            - 05시 발표
        - 제한:
            - 1일 8회 발표
                - 0200, 0500, 0800, 1100, 1400, 1700, 2000, 2300
            - 매 발표 시간 10분 이후 호출할 수 있음
                - 0200 발표 데이터를 얻으러면 02시 10분 이후 호출 가능
    - 예보 지점(nx, ny):
        - 행정동 기준 지역 지점 좌표(X, Y)
        - 위도와 경도값을 좌표로 변환하는 과정이 추가적으로 필요
            - [Lambert Conformal Conic Projection](https://en.wikipedia.org/wiki/Lambert_conformal_conic_projection)
                - 변수:
                    - 사용자 위치의 위도(latitude) 경도(longitude)
                - 상수:
                    - 중앙 경도(central meridian):
                        - 참고로 본초자오선
                    - 기준 위도(latitude at which y coordinate is to be 0 on central meridian): 38.0
                    - 지도에서 사용하는 지구의 반경(radius on Earth): 6371.00877(KM)
                    - 격자 간격: 5.0(KM)
                    - first 표준 위도(first standard parallel): 30.0
                    - second 표준 위도(second standard parallel): 60.0

                * 괄호는 위키피디아에 대응되는 용어
        - API 호출해서 구할 수 있음

- 출력:
    - 자료구분코드(category):
        - PCP(1시간 강수량)
        - PTY(강수형태)
        - POP(강수확률)
    - 예측값(fcstValue):
        - PCP:
            - x(mm)
            - 강수량 mm 단위
            - 단기 예보 연장(3시간 단위 제공)의 경우 코드 값(정성값)으로 제공:
                - 1: 약한비
                - 2: 보통비
                - 3: 강한비
        - PTY:
            - 코드에 대응
            - 없음(0), 비(1), 비/눈(2), 눈(3), 소나기(4)
        - POP:
            - x(%)
            - 강수확률
    - 예측시간(fcstTime):
        - 4일 까지는 1시간 단위로 예측
        - 연장(5일)은 3시간 단위로 예측
    - 예측일자(fcstDate)
        - 발표일자와 동일함

- 초단기예보 생성 및 업데이트:
    - 매시간 30분에 생성 10분마다 업데이트

## 임의의 위.경도를 격자로 번호로 변환하는 API

- 입력:
    - 임의 경도(lon)
    - 임의 위도(lat)

- 출력:
    - 격자 x:
        - 동서방향
    - 격자 y:
        - 남북방향

## 초단기 예보 격자자료 API

- 설명:
    - 발표시간, 발효시간을 조건으로 전국 모든 격자의 예보변수 값 조회
    - 발표시간 기준 6시간 이내의 예보를 조회

- 입력:
    - 발표시간(tmfc)
        - 설명:
            - 최신 정보로 업데이트된 시간을 의미함
            - 매 10분마다 최신 정보로 업데이트
                - 250113 테스트 결과 25011320 발표시간의 자료는 25011340 을 지나야 호출가능
        - 샘플데이터:
            - 202501051010
            - 연월일시분(KST)
        - 제한:
            - 초단기 예보와 동일한 자료를 사용함
                - 기준 시각 + 30분 == 예보 자료 생성시각
                - 기준 시각 + 45분 == API 제공 시간
                - 매 10분마다 최신 정보로 업데이트
            - 테스트 결과 여유롭게 30분은 지나서 호출하는게 좋아보임
    - 발효시각(tmef)
        - 샘플데이터:
            - 2025010511
            - 연월일시(KST)
        - 제한:
            - 기준시간 기준 6시간까지 1시간 간격
                - 발표시간 값이 '202501051010' 라면 기준시간이 2025010510이고, '2025010510', '2025010511' ... '2025010515'
    - 예보변수(vars)
        - 샘플데이터:
            - T1H(기온), UUU(동서바람성분), VVV(남북바람성분), VEC(풍향), WSD(풍속), SKY(하늘상태), LGT(낙뢰), PTY(강수형태), RN1(1시간 강수량), REH(
              상대습도)
        - PTY 코드:
            - 없음(0), 비(1), 비/눈(2), 눈(3), 빗방울(5), 빗방울눈날림(6), 눈날림(7)

- 입력 시나리오:
    - 현재 분에서 1의 자리 버리기
        - 04시 40분
    - 30분 빼기
        - 0410
    - 기준 시각 구하기
        - 04
    - 기준 시각에서 발효시간 6개 도출하기

- 출력:
    - 관측영역과 비관측영역을 모두 포함한 전국 격자 자료
        - 행정동 기준 지역 지점 좌표(X, Y)
        - 위도와 경도값을 좌표로 변환하는 과정이 추가적으로 필요
            - [Lambert Conformal Conic Projection](https://en.wikipedia.org/wiki/Lambert_conformal_conic_projection)
                - 변수:
                    - 사용자 위치의 위도(latitude) 경도(longitude)
                - 상수:
                    - 중앙 경도(central meridian):
                        - 참고로 본초자오선
                    - 기준 위도(latitude at which y coordinate is to be 0 on central meridian): 38.0
                    - 지도에서 사용하는 지구의 반경(radius on Earth): 6371.00877(KM)
                    - 격자 간격: 5.0(KM)
                    - first 표준 위도(first standard parallel): 30.0
                    - second 표준 위도(second standard parallel): 60.0

                * 괄호는 위키피디아에 대응되는 용어
        - API 호출해서 구할 수 있음
    - 비관측영역 값: -99.0

## Change History

- 20250105: 문서 생성
- 20250107: 초단기 예보 격자자료 API 추가
    - 모든 행정동(1635개 격자 구역)에 대해 1시간에 한 번씩 호출하면 일 최대호출건수 제한을 초과
    - 한 번 호출할 때 전국 모든 지역의 예보값을 알 수 있는 api로 변경
    - 매 분 호출하면 하루에 144회 호출이라 넉넉함
