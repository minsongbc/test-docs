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
현재 디바이스의 Locale 정보를 반환합니다.

#### Example
```csharp
var locale = Gaia.Locale;
```

### Offset
`string offset`
현재 디바이스의 Time offset 정보를 반환합니다.

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

### Initialize
`void Initialize(GaiaConfiguration config)`
Gaia Core SDK를 초기화 합니다. 다른 Gaia SDK들을 사용하기 위해서는 타 SDK보다 우선적으로 초기화해야합니다.

#### Example
```csharp
var config = new GaiaConfiguration
{
    AppName = Application.productName,
    AppVersion = Application.version
};

GaiaSDK.Core.Gaia.Initialize(config);
```

### ChangeEnv
`void ChangeEnv(GaiaEnv targetEnv)`
Environment를 변경합니다.

#### Example
```csharp
Gaia.ChangeEnv(GaiaEnv.Alpha); // Alpha로 변경
Gaia.ChangeEnv(GaiaEnv.QA); // QA로 변경
Gaia.ChangeEnv(GaiaEnv.Production); // Production으로 변경
```

### SetDevelopmentDeviceId
`void SetDevelopmentDeviceId(string fakeDeviceId)`
설명필요

#### Example
```csharp
Gaia.SetDevelopmentDeviceId(id);
```

### UnsetDevelopmentDeviceId
`void UnsetDevelopmentDeviceId()`
설명필요

#### Example
```csharp
Gaia.UnsetDevelopmentDeviceId();
```

### ResetGaiaData
`void ResetGaiaData()`
설명필요

#### Example
```csharp
Gaia.ResetGaiaData();
```

### GetExternalUserId
`string GetExternalUserId()`
설정한 External User Id값을 가져옵니다. 이는 외부 인증서버를 사용하기위한 메소드이며 Enable External User Id가 true로 설정되어 있어야합니다.

#### Example
```csharp
var userId = Gaia.GetExternalUserId();
```

### SetExternalUserId
`void SetExternalUserId(string externalUserId)`
External User Id를 설정합니다. 이는 외부 인증서버를 사용하기위한 메소드이며 Enable External User Id가 true로 설정되어 있어야합니다.

#### Example
```csharp
Gaia.SetExternalUserId(userId);
```

### GetExternalDeviceId
`string GetExternalDeviceId()`
설정한 External Device Id값을 가져옵니다. 이는 외부 인증서버를 사용하기위한 메소드이며 Enable External Device Id가 true로 설정되어 있어야합니다.

#### Example
```csharp
var deviceId = Gaia.GetExternalDeviceId();
```

### SetExternalDeviceId
`void SetExternalDeviceId(string externalDeviceId)`
External Device Id를 설정합니다. 이는 외부 인증서버를 사용하기위한 메소드이며 Enable External Device Id가 true로 설정되어 있어야합니다.

#### Example
```csharp
Gaia.SetExternalDeviceId(deviceId);
```

## 주의사항들
### Enable External User/Device Id
외부 인증서버를 사용하여 UserId 혹은 DeviceId가 Gaia Authentication에서 제공하는 값과 다를경우에는 Enable External User Id, Enable External Device Id를 각각 필요에 따라서 true로 설정해줘야합니다. 다만 이 값은 코드상에서 수정하는값이 아닌, 유니티 ScriptableObject로 구성되어있는 asset에서 지정하는 값이기때문에 사용자가 코드상에서 수정할 수 없습니다.
