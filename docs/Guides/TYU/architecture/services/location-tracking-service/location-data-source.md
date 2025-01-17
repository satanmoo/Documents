# Location Data Source

- 스마트폰에서 수집하는 위치관련 데이터에 대한 문서

## Content

1. [Data Format](#data-format)
2. [Expo location API](#expo-location-api)
3. [Expo location usage](#expo-location-usage)
4. [Geocoding API](#geocoding-api)
5. [Geocoding Usage](#geocoding-usage)
6. [Reference material](#reference-material)

## Data Format

### LocationObject

- coords:
    - 타입:
        - LocationObjectCoords
    - 설명:
        - 구체적인 위치 정보
- mocked:
    - 미사용
- timestamp:
    - 설명 생략
- [docs](https://docs.expo.dev/versions/latest/sdk/location/#locationobject)

### LocationObjectCoords

```json
{
  "accuracy": 10.5,
  "altitude": 15.2,
  "altitudeAccuracy": 5.0,
  "heading": 90.0,
  "latitude": 37.7749,
  "longitude": -122.4194,
  "speed": 1.5
}
```

- accuracy:
    - 위치의 불확실성 반경
    - 미터 단위
- altitude:
    - WGS 84 좌표계 기준 고도
    - 미터 단위
- altitudeAccuracy:
    - 고도 값의 정확도
    - 미터 단위
- heading:
    - 기기의 이동 방향
    - 도(degree) 단위
        - 북쪽이 0도, 동쪽이 90도, 남쪽이 180도
- latitude:
    - 위도
- longitude:
    - 경도
- speed:
    - 기기의 순간 속도
    - meter per second

- [ref](https://docs.expo.dev/versions/latest/sdk/location/#locationobjectcoords)

### LocationPermissionResponse

- PermissionResponse 타입 속성
    - PermissionStatus:
        - DENIED
        - GRANTED
        - UNDETERMINED
    - [ref](https://docs.expo.dev/versions/latest/sdk/location/#permissionresponse)
    - 이 타입 상속받아 플랫폼 별 속성을 추가해 LocationPermissionResponse 타입을 구현

- PermissionDetailsLocationAndroid:
    - accuracy:
        - 'fine' | 'coarse' | 'none'

- PermissionDetailsLocationIOS:
    - scope:
        - 'whenInUse' | 'always' | 'none'

- [ref](https://docs.expo.dev/versions/latest/sdk/location/)

## Expo location API

### Location.startLocationUpdatesAsync(taskName, options)

- 백그라운드 위치 추적
- [docs](https://docs.expo.dev/versions/latest/sdk/location/#locationstartlocationupdatesasynctaskname-options)

### LocationOptions

- [LocationOptions](https://docs.expo.dev/versions/latest/sdk/location/#locationtaskoptions):
    - accuracy:
        - 설명:
            - 오차 범위
        - 활용:
            - 100미터 이내(Balanced) 오차 범위
            - 배터리 소모와 위치 추적의 trade-off 고려했을 때 적당함
    - distanceInterval:
        - 설명:
            - 이 값으로 지정한 거리 이상 위치가 변하면 위치 정보 업데이트(전송)
            - 기본값은 정확도 옵션에 따라 달라짐. 정확도가 높을 수록 자주 업데이트
            - BALANCED 옵션의 경우 100m 이상 이동할 때 위치 업데이트 발생
    - timeInterval(Android Only):
        - 설명:
            - 최소 시간 간격
            - 이 값으로 지정한 시간마다 위치 정보 업데이트
            - 기본값은 정확도 옵션에 따라 달라짐. 정확도가 높을 수록 자주 업데이트
            - BALANCED 옵션의 경우 3초(3000 밀리 세컨드) 마다 업데이트

### LocationTaskOptions

- LocationOptions 확장으로 ios 관련 옵션을 포함한 추가 옵션이 있음

- [LocationTaskOptions](https://docs.expo.dev/versions/latest/sdk/location/#locationtaskoptions):
    - [ActivityType](https://docs.expo.dev/versions/latest/sdk/location/#activitytype):
        - 디폴트 옵션 사용
    - deferredUpdatesDistance
        - 위치 업데이트가 지연됬을 때, 마지막으로 보고(report)된 위치와 현재 위치 간에 이 정도 거리 이상 이동해야만 지연된 위치 정보를 업데이트
        - 모든 지연된 위치 정보를 매번 업데이트하지 않아도 됨
    - deferredUpdatesInterval
        - 위치 업데이트가 지연됬을 때, 마지막으로 보고(report)된 위치와 현재 위치 간에 설정된 시간 이상 경과해야 지연된 위치 정보를 업데이트
    - deferredUpdatesTimeout, foregroundService
        - 미구현

- defer 관련 처리는 버퍼와 유사하게 동작한다. 지연된 위치를 모두 업데이트하는게 아니라 미리 계산해놓고, 특정 조건을 만족하면 보고(report)하게 된다.
    - 보고(report)는 다른 시스템, 컴포넌트 등으로 위치 정보를 보내는 Task를 수행하는 것

## Expo location usage

### 안드로이드

- 정확도:
    - BALANCED
- timeInterval:
    - 추가적으로 커스텀 필요
    - BALANCED의 기본값은 3초마다 업데이트인데, 시간 기준은 무시하고 위치 기준으로만 업데이트하게 커스터마이징
    - [setIntervalMill](https://developer.android.com/reference/android/location/LocationRequest.Builder#setIntervalMillis(long))
    - [PASSIVE_INTERVAL](https://developer.android.com/reference/android/location/LocationRequest#PASSIVE_INTERVAL)
- 지연 옵션은 안 건드려도 됨
- mayShowUserSettingsDialog
    - 기본값이 true

### ios

- 정확도:
    - BALANCED
- 지연 옵션은 안 건드려도 됨
- activityType:
    - default
- [showsBackgroundLocationIndicator](https://developer.apple.com/documentation/corelocation/cllocationmanager/showsbackgroundlocationindicator/)
    - 앱이 백그라운드에서 위치서비스를 실행할 때 상태표시줄에 위치 서비스가 활성화 되어있다는 아이콘을 표시하는 지 여부
    - true
- [pausesLocationUpdatesAutomatically](https://developer.apple.com/documentation/corelocation/cllocationmanager/pauseslocationupdatesautomatically/)
    - 불필요한 위치 추적을 방지해 배터리 효율을 높일 수 있음
    - true

### common

- expo location 에서 안드로이드는 시간(time interval)기반 위치 추적 api를 제공하지만, ios는 제공하지 않음
- 일관성을 위해 안드로이드의 시간기반 위치 추적을 사용하지 않고, 두 플랫폼 모두 거리 기반 위치추적
- 위치 정보를 수집하고, 업데이트 시간을 통해 직접 체류시간을 계산
    - timestamp 속성 활용

## Geocoding API

- 설명:
    - 좌표를 주소로 변환
    - 일일 요청건수는 무제한

- 제한:
    - 개발키: 개발을 목적으로하며 유효기간은 6개월, 최대 3회 연장가능합니다.
    - 운영키: 서비스 운영을 목적으로하며 유효기간은 2년, 유효기간 연장 신청을 통해 관리자 심사 후 연장가능합니다.
        - 개발키 최대 연장 후 운영키로 변경

- 입력:
    - service:
        - 설명:
            - 서비스 이름
            - 기본값은 "address"로 바꿀필요 없음
    - version:
        - 설명:
            - 요청 서비스 버전
            - 기본값은 "2.0"으로 바꿀필요 없음
    - request:
        - 설명:
            - 서비스 오퍼레이션
            - 좌표를 주소로 변환하기 때문에 "GetAddress" 값을 입력
    - point:
        - 설명:
            - 좌표
        - 포맷:
            - x,y
            - 경도,위도
        - 샘플데이터:
            - "126.978275264,37.566642192"
    - crs:
        - 설명:
            - 좌표계
        - 샘플데이터:
            - "epsg:4326"
            - WGS84 경위도 좌표계 ExpoLocation의 위도,경도 좌표계와 동일하게 지정
            - WGS84 경위도 좌표계에 대응하는 값이 "epsg:4326"
    - type:
        - 설명:
            - 검색 주소 유형
            - 도로명주소, 지번주소 모두 요청가능
        - 샘플데이터:
            - "BOTH"
            - 도로명주소, 지번주소 모두 포함
    - zipcode:
        - 설명:
            - 우편 번호 반환여부
    - callback:
        - 설명:
            - format 값이 "json" 인 경우 콜백함수 지원

### example

- input

```shell
https://api.vworld.kr/req/address?service=address&request=getAddress&version=2.0&crs=epsg:4326&point=126.996777777777,37.5176055555555&format=json&type=both&zipcode=true&simple=false&key=<myapikey>
```

- output
- 도로명 주소는 없는 곳이 있음

```json
{
  "response": {
    "service": {
      "name": "address",
      "version": "2.0",
      "operation": "getAddress",
      "time": "9(ms)"
    },
    "status": "OK",
    "input": {
      "point": {
        "x": "126.9706519",
        "y": "37.5841367"
      },
      "crs": "epsg:4326",
      "type": "both"
    },
    "result": [
      {
        "zipcode": "03047",
        "type": "parcel",
        "text": "서울특별시 종로구 궁정동 12-1",
        "structure": {
          "level0": "대한민국",
          "level1": "서울특별시",
          "level2": "종로구",
          "level3": "",
          "level4L": "궁정동",
          "level4LC": "1111010300",
          "level4A": "청운효자동",
          "level4AC": "1111051500",
          "level5": "12-1",
          "detail": ""
        }
      },
      {
        "zipcode": "03047",
        "type": "road",
        "text": "서울특별시 종로구 자하문로 92 (궁정동,청운효자동주민센터)",
        "structure": {
          "level0": "대한민국",
          "level1": "서울특별시",
          "level2": "종로구",
          "level3": "궁정동",
          "level4L": "자하문로",
          "level4LC": "3100012",
          "level4A": "청운효자동",
          "level4AC": "1111051500",
          "level5": "92",
          "detail": "청운효자동주민센터"
        }
      }
    ]
  }
}
```

- [ref](https://www.vworld.kr/dev/v4dv_geocoderguide2_s002.do)

## Geocoding usage

- 위,경도를 주소로 변환
    - reverse geocoding

### solution 1

- 미리 격자에 대한 대표 주소를 저장해놓는 방식
- 위치 정보를 수집해 격자로 변환
- 기상청에서 제공하는 {격자쌍:위,경도} 는 1:1 대응이고, 이 위,경도를 geocoding 한 결과 {위,경도: 주소} 를 조회
    - Lambert-Conformal map projection 으로 역 역산도 코드 예시 제공됨
        - api 호출할 필요없이 내부에서 계산 가능
        - 위도,경도 -> 격자
            - many to one 관계
        - 격자 -> 위도,경도 (역으로 변환)
            - one to one 관계
        - 어떤 위도,경도를 격자로 변환하고 다시 역으로 변환했을 때 원래의 위도,경도 값이 나오지 않을 수 있음

### solution 2

- 자체 지도 db 구축하고
- 검색엔진
- ... 불가능

## Reference material

### android

- expo-location
    - android
        - src
            - main
                - java
                    - expo
                        - modules
                            - location

- 이 모듈 참고
    - 함수 파라매터로 사용하는 인자는 LocationArgument.kt 확인
    - expo location 함수와 대응되는 네이티브 모듈은 LocationModule.kt 참고

- distance 값에 따라 위치 업데이트 발생
- 최종적으로 구글의 안드로이드 위치 서비스인 Google Play Services에서 제공하는 Location API 사용
- [ref](https://developers.google.com/android/reference/com/google/android/gms/location/LocationRequest?hl=en#getMinUpdateDistanceMeters())

- buildLocationParamsForAccuracy, prepareLocationRequest 함수는 LocationOptions을 어떻게 사용하는지 보여준다.

```kotlin
private fun buildLocationParamsForAccuracy(accuracy: Int): LocationParams {
    return when (accuracy) {
        LocationModule.ACCURACY_LOWEST -> LocationParams(
            accuracy = LocationAccuracy.LOWEST,
            distance = 3000f,
            interval = 10000
        )
        LocationModule.ACCURACY_LOW -> LocationParams(
            accuracy = LocationAccuracy.LOW,
            distance = 1000f,
            interval = 5000
        )
        LocationModule.ACCURACY_BALANCED -> LocationParams(
            accuracy = LocationAccuracy.MEDIUM,
            distance = 100f,
            interval = 3000
        )
        LocationModule.ACCURACY_HIGH -> LocationParams(
            accuracy = LocationAccuracy.HIGH,
            distance = 50f,
            interval = 2000
        )
        LocationModule.ACCURACY_HIGHEST -> LocationParams(
            accuracy = LocationAccuracy.HIGH,
            distance = 25f,
            interval = 1000
        )
        LocationModule.ACCURACY_BEST_FOR_NAVIGATION -> LocationParams(
            accuracy = LocationAccuracy.HIGH,
            distance = 0f,
            interval = 500
        )
        else -> LocationParams(accuracy = LocationAccuracy.MEDIUM, distance = 100f, interval = 3000)
    }
}
```

```kotlin
internal fun prepareLocationRequest(options: LocationOptions): LocationRequest {
    val locationParams = mapOptionsToLocationParams(options)

    return LocationRequest.Builder(locationParams.interval)
        .setMinUpdateIntervalMillis(locationParams.interval)
        .setMaxUpdateDelayMillis(locationParams.interval)
        .setMinUpdateDistanceMeters(locationParams.distance)
        .setPriority(mapAccuracyToPriority(options.accuracy))
        .build()
}
```

- prepareLocationRequest 함수는 [LocationRequest](https://developer.android.com/reference/android/location/LocationRequest)
  를 생성할 때 옵션값을 사용한다.
- 직접 LocationOptions의 timeInterval을 설정하는 경우 업데이트 간격을 설정할 수 있다.
    - 최소 업데이트 주기
    - 최대 지연 시간 설정
        - 그룹화해서 배치 처리
        - 최대 지연 시간 안에 여러 개의 위치 업데이트가 발생해도 지연시간이 지나고 묶어서 처리

```kotlin
internal fun prepareLocationRequest(options: LocationOptions): LocationRequest {
    val locationParams = mapOptionsToLocationParams(options)

    return LocationRequest.Builder(locationParams.interval)
        .setMinUpdateIntervalMillis(locationParams.interval)
        .setMaxUpdateDelayMillis(locationParams.interval)
        .setMinUpdateDistanceMeters(locationParams.distance)
        .setPriority(mapAccuracyToPriority(options.accuracy))
        .build()
}
```

- deferLocations 함수는 지연된 위치 정보를 누적해서 계산한다.

```kotlin
private fun deferLocations(locations: List<Location>) {
    val size = mDeferredLocations.size
    var lastLocation = if (size > 0) mDeferredLocations[size - 1] else mLastReportedLocation
    for (location in locations) {
        if (lastLocation != null) {
            mDeferredDistance += abs(location.distanceTo(lastLocation)).toDouble()
        }
        lastLocation = location
    }
    mDeferredLocations.addAll(locations)
}
```

- shouldReportDeferredLocations 함수는 지연된 위치 정보를 보고할지 결정하는 조건 함수다.

```kotlin
private fun shouldReportDeferredLocations(): Boolean {
    val task = mTask ?: return false
    if (mDeferredLocations.size == 0) {
        return false
    }
    if (!mIsHostPaused) {
        // Don't defer location updates when the activity is in foreground state.
        return true
    }
    val oldestLocation = mLastReportedLocation ?: mDeferredLocations[0]
    val newestLocation = mDeferredLocations[mDeferredLocations.size - 1]
    val options: Arguments = MapHelper(task.options)
    val distance = options.getDouble("deferredUpdatesDistance")
    val interval = options.getLong("deferredUpdatesInterval")
    return newestLocation.time - oldestLocation.time >= interval && mDeferredDistance >= distance
}
```

### ios

- expo-location
    - ios

- LocationAccuracy enum에서 accuracy 옵션의 구체적인 수치를 확인할 수 있음
- [ref](https://developer.apple.com/documentation/corelocation/kcllocationaccuracyhundredmeters)

```swift
// Copyright 2023-present 650 Industries. All rights reserved.

import ExpoModulesCore

internal enum LocationAccuracy: Int, Enumerable {
  case lowest = 1
  case low = 2
  case balanced = 3
  case high = 4
  case highest = 5
  case bestForNavigation = 6

  func toCLLocationAccuracy() -> CLLocationAccuracy {
    switch self {
    case .lowest:
      return kCLLocationAccuracyThreeKilometers
    case .low:
      return kCLLocationAccuracyKilometer
    case .balanced:
      return kCLLocationAccuracyHundredMeters
    case .high:
      return kCLLocationAccuracyNearestTenMeters
    case .highest:
      return kCLLocationAccuracyBest
    case .bestForNavigation:
      return kCLLocationAccuracyBestForNavigation
    }
  }
}
```

- LocationOptions의 distanceInterval 속성은 distanceFilter 대응된다.
- [ref](https://developer.apple.com/documentation/corelocation/cllocationmanager/distancefilter/)

- deferLocations 함수는 지연된 위치 정보를 누적해서 계산한다.

```swift
- (void)deferLocations:(NSArray<CLLocation *> *)locations
{
  CLLocation *lastLocation = _deferredLocations.lastObject ?: _lastReportedLocation;

  for (CLLocation *location in locations) {
    if (lastLocation) {
      _deferredDistance += [location distanceFromLocation:lastLocation];
    }
    lastLocation = location;
  }
  [_deferredLocations addObjectsFromArray:locations];
}
```

- shouldReportDeferredLocations 함수는 지연된 위치 정보를 보고할지 결정하는 조건 함수다.

```swift
- (BOOL)shouldReportDeferredLocations
{
  if (_deferredLocations.count <= 0) {
    return NO;
  }
  UIApplicationState appState = [[UIApplication sharedApplication] applicationState];

  if (appState == UIApplicationStateActive) {
    // Don't defer location updates when app is in foreground state.
    return YES;
  }

  CLLocation *oldestLocation = _lastReportedLocation ?: _deferredLocations.firstObject;
  CLLocation *newestLocation = _deferredLocations.lastObject;
  NSDictionary *options = _task.options;
  CLLocationDistance distance = [self numberToDouble:options[@"deferredUpdatesDistance"] defaultValue:0];
  NSTimeInterval interval = [self numberToDouble:options[@"deferredUpdatesInterval"] defaultValue:0];

  return [newestLocation.timestamp timeIntervalSinceDate:oldestLocation.timestamp] >= interval / 1000.0 && _deferredDistance >= distance;
}
```

## Change History

- 20250115: reverse geocoding API 추가
    - 알림 메시지에 주소를 표시하기 위함
