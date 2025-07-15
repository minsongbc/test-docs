***

title: Gaia Mobile Remote Notification 연동 가이드
description: 유니티 엔진에서 Android/iOS 환경에 Remote Notification을 연동하기 위한 가이드
keyword: 유니티, android, ios, remote notification, notification, 알림, 푸시 알림
------------------------------------------------------

# Gaia Notification
Gaia Notification은 유니티 엔진에서 Remote Notification을 손쉽게 구현할 수 있도록 제공하는 SDK 입니다.

## Interfaces

### Initialize
Gaia Notification을 사용하기 위해서는 우선 Gaia Core가 Initialize 된 후, GaiaNotification Initialize를 진행해야합니다.

```csharp
var config = new GaiaConfiguration
{
    AppName = Application.productName,
    AppVersion = Application.version
};

// Gaia SDK를 초기화
GaiaSDK.Core.Gaia.Initialize(config); // 일반적으로 이런 방식으로 Gaia Core를 초기화함
GaiaSDK.Notification.GaiaNotification.Initialize(); // 여기서 Notification 초기화
```

Initialize를 호출하게 되면 앱 설치 초기에 Notification 권한 팝업이 등장할 수 있습니다.

Initialize는 두가지 방식으로 초기화할 수 있습니다.

`void Initialize()`
단순한 형태의 초기화입니다.

`void Initialize(Action<bool> onAuthorizeRequest)`
호출시에 os에서 시스템 권한 프롬프트가 등장했을때 권한 허용을 허용했는지, 거부했는지 알수있는 콜백을 받습니다. 이는 앱 설치 초기에 권한 요청을 거부했을때 팝업을 띄우는 경우에 사용하는걸 고려해서 만들었으며 이후에 os에서 더 이상 팝업이 등장하지 않을때는 true로 콜백이 오게 됩니다.
꼭 필요한 기능이 아니라면 Initialize로 충분합니다.

### RegisterMessageReceived
`void RegisterMessageReceived(System.EventHandler<Firebase.Messaging.MessageReceivedEventArgs> onMessageReceived)`
Notification이 도착했을때 호출되는 이벤트를 등록합니다.

### IsPermissionGranted
`bool IsPermissionGranted()`
현재 os에서 Notification 권한이 있는지 알 수 있습니다.

### GetPushToken
`string GetPushToken()`
PushToken를 가져옵니다.

### IsTokenInitialized()
`bool IsTokenInitialized()`
토큰이 초기화되었는지 유무를 확인합니다.

### RegisterPushToken
`void RegisterPushToken(GaiaCallback<bool> gaiaCallback)`
PushToken을 Notification 서버에 등록합니다. 호출하면 자동으로 서버에 등록하며, 서버에 등록했을때 성공, 실패 유무를 콜백으로 받을 수 있습니다. FCM Token의 만료 및 변경에 대응하기 위해 앱이 켜질때마다 꼭 등록해주어야 합니다.

### ActivatePushToken
`void ActivatePushToken(GaiaCallback<bool> gaiaCallback)`
Push Token을 Gaia Notification 서버로부터 활성화 하여 다시 푸시를 받을 수 있습니다.

### DeactivatePushToken
`void DeactivatePushToken(GaiaCallback<bool> gaiaCallback)`
Push Token을 Gaia Notification 서버로부터 비활성화 하여 푸시를 받지 않습니다.

### SetUserTag, SetUserTags
`void SetUserTag(string tagKey, string tagValue, GaiaCallback<bool> gaiaCallback)`
Push Token과 연결된 사용자의 단일 태그 (key, value) 를 설정합니다.

`void SetUserTags(Dictionary<string, string> tags, GaiaCallback<bool> gaiaCallback)`
Push Token과 연결된 사용자의 단일 태그들을 설정합니다.

### OpenNotificationSettings
`void GaiaNotification.OpenNotificationSettings()`
os에 알림 설정창으로 이동합니다. 이는 앱 내에서 권한을 요청하는것이 아닌 직접 설정창으로 이동하는 방식입니다.

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
