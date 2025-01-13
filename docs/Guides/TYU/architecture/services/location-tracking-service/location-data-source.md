# Location Data Source

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
    - WGS 84 기준 고도
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

## API

### Location.startLocationUpdatesAsync(taskName, options)

- 백그라운드 위치 추적
- [docs](https://docs.expo.dev/versions/latest/sdk/location/#locationstartlocationupdatesasynctaskname-options)

## Options

- 백그라운드 위치 수집
- 옵션으로 제어 가능

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

## Usage

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

## 참고 소스 코드

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
