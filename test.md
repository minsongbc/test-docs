# GAIA Notification Package for UNITY
**Gaia Notification Package for Unity** 는 Firebase Cloud Messaging을 기반으로 구현되어있으며 손쉽게 사용할 수 있도록 간소화한 SDK입니다.
- 기기/어플리케이션 알림 허용/차단을 관리합니다.
- Firebase로부터 발급되는 FCM(Firebase Cloud Message) Token을 자동으로 받아옵니다.
- FCM Token을 Gaia Notification 서버에 업로드합니다.
- 푸시 알림이 수신되었을 경우, 제대로 수신 되었는지, 유저가 어떤 반응을 보였는지를 트래킹 하기 위해 Gaia Notification 서버 API를 호출합니다.

> [!CAUTION]
> 해당 SDK는 Firebase Cloud Messaging과 Unity Mobile Notification의 의존성이 포함되어있지만 package.json에서 별도로 의존성 관리를 하고있지 않기때문에 해당부분을 수동으로 추가하여 주셔야합니다.
>
> package.json에서는 별도로 의존성을 추가하지 않은 이유는 외부 라이브러리 (e.g., Saygames)끼리 서로 의존성 충돌이 발생하는 경우가 있어 Inhouse SDK에서 별도로 의존성을 제거한 상황입니다.

## Initialize

> Platform Team으로 부터 발급받은 AppIdentifier와 AppSecretKey는 Editor 메뉴 > Gaia > Configuration 를 통해 입력합니다.

Gaia SDK는 GaiaSDK namespace 하위에 정의 되어있습니다.

```csharp
var config = new GaiaConfiguration
{
    AppName = "DevBed",
    AppVersion = "0.9.4"
};

// Gaia SDK를 초기화
GaiaSDK.Core.Gaia.Initialize(config); // GaiaConfiguration
GaiaSDK.Notification.GaiaNotification.Initialize();
```
Initialize를 호출하면 os의 알림 권한요청 팝업을 띄웁니다. Android는 2회까지는 띄울 수 있고(사용자가 거부했을때 추가적으로 한번 더), iOS는 허용/거부 상관없이 딱 한번만 띄우고 이후부터는 os에서 띄우지 못하도록 막혀있습니다.

알림 권한 요청팝업이 띄워져있는 동안에는 유니티가 동작하지 않습니다.

AppName은 Gaia Console의 앱 이름과 꼭 매칭되지 않아도 상관없습니다.

Initialize는 아래와 같이 콜백을 추가해서 사용할 수도 있습니다.

```csharp
GaiaSDK.Notification.GaiaNotification.Initialize((isGranted) =>
{
  // implement here
});
```

현재 permission이 granted 되었는지 콜백으로 처리 가능하며, 알림 권한요청 팝업의 결과에 따라서 콜백을 제공하지만, 이후에 os에서 별도로 권한요청 팝업이 나오지 않더라도 콜백은 여전히 호출하기때문에 해당부분을 유의해서 구현하셔야 합니다.

## SDK Initialization

https://firebase.google.com/docs/unity/setup

Firebase SDK가 게임 시작 시에 제대로 초기화 되기 위해서는 Assets/google-services.json 파일이 존재해야 합니다. 이 파일은 Firebase 콘솔에서 새 앱을 정의할 때 받을 수 있으며, 프로젝트의 지정된 위치에 존재해야 합니다. 만약 빌드 환경에 따라 다른 Firebase 앱으로 간주되길 원하는 경우, 빌드 이전 시점에 이름 바꾸기를 통해 파일을 바꿔치기 해야 합니다.

```
./
├ Assets/
│ └ google-services.json
└ Packages/
  ├ com.bagelgames.gaia.core/
  ├ com.google.firebase.messaging/
  ├ com.google.firebase.app/
  └ com.google.external-dependency-manager/
```

## 자체 Authorization 사용시 초기화.

자체적인 인증 시스템이 있어서 Gaia Authentication을 사용하지 않는 경우, Gaia Core에서 Enable Extenral User Id를 체크하고, External User Id를 Gaia에 등록하여 설정할 수 있습니다.

```csharp
GaiaSDK.Core.Gaia.SetExternalUserId("string");
```

### PushToken Registration

Firebase에서 가져온 FCM Token이 Gaia 서버에 등록되어야 비로소 알림 대상으로 지정될 수 있습니다. 토큰은 Gaia Notification 에서 보관하고 있으므로, 사용 시에는 별도 작업 없이 RegisterToken()만 호출하면 작동합니다.

반대로 더 이상 알림을 받지 않고자 할때는 UnregisterToken()을 호출하면 해제됩니다.

### Get Started

```cs
GaiaSDK.Core.Gaia.Initialize(config);
GaiaSDK.Notification.GaiaNotification.Initialize(); // none callback

GaiaAuthentication.GetOAuthToken(IdType.GUEST, "", "",
    result =>
    {
        if (result.TryGetValue(out var token, out var error))
        {
            GaiaNotification.RegisterPushToken(delegate {});
        }
        else
        {
            // Error handling...
        }
    }
)
```

### Interface
#### `void GaiaNotification.Initialize()`
#### `void GaiaNotification.Initialize(Action<bool> onAuthorizeRequest)`

Gaia Notification 모듈을 초기화 합니다.
호출시 os의 알림 권한요청 팝업을 띄웁니다. 권한요청 팝업이 띄워져 있을때는 유니티가 동작하지 않으며, 허용/거부에 따라서 콜백처리를 추가적으로 구현할 수 있습니다.

> [!WARNING]  
> os 권한 요청은 아래 문서에 정리된 내용대로 제한적으로만 제공하고, 이후에는 앱을 삭제하지 않으면 띄울수 없도록 되어있습니다.
>
> [Android: If the user selects the don't allow option, your app can't send notifications unless it qualifies for an exemption. All notification channels are blocked, except for a few specific roles. This is similar to the behavior that occurs when the user manually turns off all notifications for your app in system settings.](https://developer.android.com/develop/ui/views/notifications/notification-permission#user-select-dont-allow)
>
> [iOS: The first time your app makes this authorization request, the system prompts the person to grant or deny the request and records that response. Subsequent authorization requests don’t prompt the person.](https://developer.apple.com/documentation/usernotifications/asking-permission-to-use-notifications#Explicitly-request-authorization-in-context)

#### `bool GaiaNotification.IsPermissionGranted()`

기기 OS에서 알림이 활성화 되어 있는지를 확인합니다.

Android의 경우 android.permission.POST_NOTIFICATIONS를 통해 값을 가져옵니다. (sdk >= 33 일때만)
iOS의 경우엔 AuthorizationStatus를 통해 구분합니다.

#### `void GaiaNotification.RequestNotificationPermission()`
#### `void GaiaNotification.RequestNotificationPermission(Action<bool> onAuthorizeRequest)`

OS에 알림 활성화 요청을 보냅니다.

시스템 팝업이 발생하며, 유저는 이 팝업에서 알림을 활성화할수도, 거부할 수도 있습니다.

만약 알림이 이미 활성화 되어 있다면, 요청을 보내지 않습니다.

Initialize에서 기본적으로 Request를 우선적으로 제공하지만, Initialize보다 먼저 권한을 요청하고 싶을때 사용하시면 됩니다. 마찬가지로 permission 결과에 따른 콜백을 제공합니다.

#### `void GaiaNotification.OpenNotificationSettings()`

Android의 경우에는 앱의 시스템 알림설정으로, iOS는 앱 설정으로 이동합니다. 위에서 설명한대로 Permission Request는 제한적으로 보통 한번만 띄울 수 있기때문에 이외의 경우에 시스템 알림허용을 권장하고 싶을때 설정창을 띄워주는 UX를 제공합니다.

> [!CAUTION]
> Android에서 FCM과 같이 사용하는 경우에 Project Settings - Mobile Notifications에서 Use Custom Activity를 활성화 해주시고 `com.google.firebase.MessagingUnityPlayerActivity` 를 activity로 지정해주셔야 정상적으로 동작합니다.

#### `RegisterMessageReceived(System.EventHandler<Firebase.Messaging.MessageReceivedEventArgs> onMessageReceived)`

커스텀 MessageReceived 이벤트를 등록합니다.

#### `void GaiaNotification.RegisterPushToken(GaiaCallback<bool>)`

Push Token을 Gaia Notification 서버에 전송하여 푸시를 받을 수 있도록 토큰을 등록합니다.
이 메서드는 FCM Token의 만료 및 변경에 대응하기 위해 **어플리케이션이 켜질 때마다** 호출되어야 합니다.


#### `void GaiaNotification.DeactivatePushToken(GaiaCallback<bool>)`

Push Token을 Gaia Notification 서버로부터 비활성화 하여 더 이상 푸시를 받지 않습니다.

#### `void GaiaNotification.ActivatePushToken(GaiaCallback<bool>)`

Push Token을 Gaia Notification 서버로부터 재 활성화 하여 다시 푸시를 받을 수 있습니다.

#### `void GaiaNotification.SetUserTag(string key, string value, GaiaCallback<bool>)`

Push Token과 연결된 사용자의 단일 태그 (key, value) 를 설정합니다.

Push Token과 연결된 사용자의 단일 태그들을 설정합니다.
