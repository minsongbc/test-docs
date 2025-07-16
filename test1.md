***

title: Gaia Core 연동 가이드
description: 유니티 프로젝트에 Inhouse SDK의 기본인 Gaia Core 가이드
keyword: 유니티, Gaia, Gaia Core, SDK
------------------------------------------------------

# Gaia Core
Gaia Core는 사내 유니티 프로젝트에서 사용하는 다양한 Gaia 파생형 SDK들의 기본 골자를 담당하고 있습니다.

## Interfaces



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
