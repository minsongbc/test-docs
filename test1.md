***

title: Gaia Core 연동 가이드
description: 유니티 프로젝트에 Inhouse SDK의 기본인 Gaia Core 가이드
keyword: 유니티, Gaia, Gaia Core, SDK
------------------------------------------------------

# Gaia Core
Gaia Core는 사내 유니티 프로젝트에서 사용하는 다양한 Gaia 파생형 SDK들의 기본 골자를 담당하고 있습니다.

## Interfaces

### IsFirstRun
`bool IsFirstRun`
설치 후 첫번째 실행인지 확인하는 정보를 반환합니다. 삭제/재설치를 감지하지는 못합니다.

#### Example
```csharp
var isFirstRun = Gaia.IsFirstRun;
```

### CurrentEnv
`GaiaEnv CurrentEnv`
현재 환경정보를 반환합니다. Gaia에서는 Alpha, QA, Production 타입을 지원합니다.

#### Example
```csharp
var environment = Gaia.CurrentEnv;
```

### AppIdentifier
`string AppIdentifier`
App Identifier를 반환합니다.

#### Example
```csharp
var identifier = Gaia.AppIdentifier;
```

### AppName
`string AppName`
GaiaConfiguration에 정의된 App Name을 반환합니다. 정의되어있지 않다면 AppIdentifier 정보를 반환합니다.

#### Example
```csharp
var name = Gaia.AppName;
```

### AppVersion
`string AppVersion`
GaiaConfiguration에 정의된 App Version을 반환합니다. 정의되어있지 않다면 "0.0.0" 을 반환합니다.

#### Example
```csharp
var version = Gaia.AppVersion;
```

### DeviceUniqueIdentifier
`string DeviceUniqueIdentifier`
세부적인 내용 작성필요.

#### Example
```csharp
var uniqueIdentifier = Gaia.DeviceUniqueIdentifier;
```

### DeviceType
`DeviceType DeviceType`
세부적인 내용 작성필요.

#### Example
```csharp
var type = Gaia.DeviceType;
```

### Locale
`string Locale`
Locale 정보를 반환합니다.

#### Example
```csharp
var locale = Gaia.Locale;
```

### Offset
`string offset`
Time offset 정보를 반환합니다.

#### Example
```csharp
var offset = Gaia.Offset;
```

### AppSecret
`string AppSecret`
App Secret Key를 반환합니다.

#### Example
```csharp
var secret = Gaia.AppSecret;
```

### Settings
`Settings Settings`
SDK 세팅 정보를 가져옵니다.

#### Example
```csharp
var settings = Gaia.Settings;
```

### AccessToken
`string AccessToken`
AccessToken을 반환합니다.

#### Example
```csharp
var token = Gaia.AccessToken;
```

### RefreshToken
`string RefreshToken`
RefreshToken을 반환합니다.

#### Example
```csharp
var token = Gaia.RefreshToken;
```

### IsExpiredAccessToken
`bool IsExpiredAccessToken`
AccessToken이 만료되었는지 정보를 반환합니다.

#### Example
```csharp
var isExpired = Gaia.IsExpiredAccessToken;
```

### IsSessionActive
`bool IsSessionActive`
현재 세션이 활성화 되어있는지 정보를 반환합니다.

#### Example
```csharp
var isActive = Gaia.IsSessionActive;
```

### IsSupportOfflineMode
`bool IsSupportOfflineMode`
Offline Mode를 지원하는지 유무를 반환합니다.

#### Example
```csharp
var isSupportOffline = Gaia.IsSupportOfflineMode;
```

### EnableExternalDeviceId
`bool EnableExternalDeviceId`
External Device Id를 지원하는지 유무를 반환합니다. 일반적인 프로젝트는 GaiaAuthentication을 사용하기때문에 외부 정보를 사용하지 않겠지만 오래된 프로젝트 혹은 별도로 자체적인 Device Id를 사용할 수 있습니다.

#### Example
```csharp
var enableExternalDeviceId = Gaia.EnableExternalDeviceId;
```

### EnableExternalUserId
`bool EnableExternalUserId`
External User Id를 지원하는지 유무를 반환합니다. 일반적인 프로젝트는 GaiaAuthentication을 사용하기때문에 외부 정보를 사용하지 않겠지만 오래된 프로젝트 혹은 별도로 자체적인 유저 인증서버의 User Id를 사용할 수 있습니다.

#### Example
```csharp
var enableExternalUserId = Gaia.EnableExternalUserId;
```

## 주의사항들
### Firebase
Gaia Notification은 내부적으로 FCM을 사용하고 있습니다. Firebase SDK가 제대로 초기화 되기 위해서는 Assets/google-services.json 파일이 존재해야 합니다.

### 자체 Authorization 사용
Gaia Authentication을 사용하지 않고, 자체적인 인증 서버를 사용한다면 Gaia Core의 GaiaConfiguration(유니티 에셋)에서 `Enable External User Id` 설정을 체크 후, 아래와 같이 External User Id를 설정해야합니다.

```csharp
GaiaSDK.Core.Gaia.SetExternalUserId("string");
```

### 팝업 권한요청 제한
android와 ios에서는 유저들에게 권한 요청팝업을 지속적으로 노출시켜 UX를 방해하는것을 제한하기 위하여 1~2회 이후에는 더 이상 권한 요청 팝업을 띄우지 못하게 os레벨에서 제한하고 있습니다.

### Android의 Mobile Notification FCM 설정
Unity Mobile Notification에서 Android의 경우에는 FCM 설정을 위하여 Project Settings - Mobile Notifications에서 Use Custom Actitivty를 활성화 해야하고, `com.google.firebase.MessagingUnityPlayerActivity` 를 activity로 지정해주셔야 정상적으로 동작합니다.
