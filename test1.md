***

title: Android, iOS 웹뷰에서 딥링크 열기
description: 각 딥링크 유형의 특징과 차이점을 자세히 알아보고, Android와 iOS 웹뷰에서 딥링크로 국내 카드앱·은행앱으로 이동하는 예시를 살펴볼게요.
keyword: 딥링크, 모바일, URL 스킴, 웹뷰, android, ios
publishedAt: 2023-3-27
thumbnailSrc: https://static.toss.im/assets/payments/contents/payments-thumb-7.jpg
----------------------------------------------------------------------------------

# Android, iOS 웹뷰에서 딥링크 열기

2023년 7월 5일 ・ 읽는 시간 11분

딥링크, 커스텀 링크, App Link… 이게 다 뭔가요? 네이티브 앱 개발자라면 한 번쯤 들어봤을 용어인데요. 이번 포스트에서는 각 딥링크 유형의 특징과 차이점을 자세히 알아보고, Android와 iOS 웹뷰에서 딥링크로 국내 카드앱·은행앱으로 이동하는 예시를 살펴볼게요.

## 딥링크란?

웹링크가 사용자를 특정 웹사이트로 이동시키듯이, 딥링크는 사용자를 특정 앱으로 이동시켜서 원하는 화면을 보여주거나, 사용자 액션을 유도해요. 예를 들어, 사용자가 온라인 쇼핑몰에서 결제 수단으로 토스페이를 선택해요. 그럼 위 그림에 있는 왼쪽 화면이 나오고 \*\*‘다음’\*\*을 누르면 토스 앱의 결제 페이지로 이동하죠. 딥링크를 사용했어요! 앱의 사용자를 늘리거나 마케팅 캠페인에 굉장히 유용해요.

사용자 입장에서는 친숙하지만, 개발자는 딥링크를 구현할 때 신경 쓸 게 많아요. 웹에서 앱을 이동시킬 땐 어떤 설정이 필요한지, 사용자 폰에 앱이 설치되어 있는지, 안 되어 있으면 사용자를 어디로 이동시킬지 모두 구현해야 돼요.

## 딥링크의 유형

### 커스텀 스킴(Custom Scheme)

커스텀 스킴은 가장 오래되었고 널리 사용되는 딥링크 유형입니다. Android, iOS에서 모두 사용할 수 있어요. 커스텀 스킴은 ‘앱 링크’, ‘URI 스킴’으로 불리기도 해요.

커스텀 스킴은 앱 스킴, 호스트, 패스(Path), 파라미터로 구성되었어요. 앱 스킴은 이동하고 싶은 앱을 특정하고, 패스는 앱에서 들어가고 싶은 페이지를 특정해요. 예를 들어, 토스앱의 스킴은 `supertoss`예요. 그럼 토스앱의 결제 페이지(`pay`)로 이동하는 링크는 `supertoss://toss/pay`가 되겠죠.

그러나 커스텀 스킴에는 서로 다른 앱에서 같은 스킴을 사용할 수 있다는 문제가 있어요. 앱이 스킴을 등록할 때 이미 있는 스킴인지 아닌지를 확인할 수가 없거든요. Android에서는 같은 스킴을 가진 앱이 두 개가 있다면 “어떤 앱을 열래?”라고 묻는 선택 UI가 뜨지만, iOS에서는 이런 문제를 해결할 방법이 없어요.

### App Link와 Universal Link

그래서 커스텀 스킴의 한계를 보완하기 위해 2015년도에 iOS에서 Universal Link, Android에서 App Link를 출시했어요. Android, iOS에서 가장 추천하는 딥링크 방식이에요.

커스텀 스킴과 달리 App Link, Universal Link는 표준 웹링크 형태예요. 예를 들어 내 서비스가 웹에서 `www.my-service.com`에 운영되고 있다면, 이 링크를 그대로 앱의 딥링크로 사용하는 거죠. 사용자의 폰에 앱이 없다면 폰 브라우저에서 웹페이지가 그대로 열려요.

그러나 App Link와 Universal Link는 사용자의 액션으로만 작동해요. 스크립트로 클릭을 유발하면 앱으로 이동하지 않고, 그대로 웹 브라우저에서 링크가 열려요. 그래서 여전히 커스텀 스킴 URL와 함께 사용되고 있어요.

### Intent 스킴

Android 환경에서는 커스텀 스킴, App Link와 사용되는 또 다른 딥링크가 있어요. 바로 Intent 스킴이죠. App Link가 개발되기 전에 Android 웹뷰에서 사용됐어요. iOS에서는 Intent 스킴을 사용하면 오류가 나요.

```plain theme="grey"
intent:
   HOST/URI-path // Optional host
   #Intent;
      package=\[string\];
      action=\[string\];
      category=\[string\];
      component=\[string\];
      scheme=\[string\];
   end;
```

Intent 스킴은 위와 같이 상당히 복잡한 형태를 가지고 있어요. 앱의 스킴, 패키지 등 많은 정보를 담고 있죠. 복잡하지만 개발자 입장에서는 Intent 스킴에서 제공하는 정보를 가공해서 다양한 방식으로 앱을 열 수 있죠. 예를 들어 앱이 설치되지 않았거나 invalid한 상태인 것을 확인하고 앱 패키지 정보로 플레이 스토어를 열 수 있어요.

아래 표는 지금까지 알아본 딥링크 유형을 정리하고 있어요. 더 자세한 내용은 링크된 Android, iOS 공식 문서를 참고하세요.

| 딥링크 유형                                                                    | Android                                                                  | iOS                                                                                            | 특징                                                             | 예시                                |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ----------------------------------- |
| 커스텀 스킴                                                                    | [O](https://developer.android.com/training/app-links/deep-linking?hl=ko) | [O](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app) | 널리 사용되지만 다른 앱에서 같은 스킴을 사용할 수 있음           | my-app://host/path                  |
| [App Link](https://developer.android.com/studio/write/app-link-indexing?hl=ko) | O                                                                        | X                                                                                              | Google에서 출시한 Android 전용 딥링크                            | https://host/path                   |
| [Universal Link](https://developer.apple.com/ios/universal-links/)             | X                                                                        | O                                                                                              | Apple에서 출시한 iOS 전용 딥링크이며 macOS, watchOS에서도 사용함 | https://host/path                   |
| [Intent 스킴](https://developer.chrome.com/docs/multidevice/android/intents/)  | O                                                                        | X                                                                                              | 스킴, 패키지 등 많은 정보를 담고 있음                            | intent://path#intent;scheme;package |

## 웹뷰에서 앱으로 이동하기

지금까지 다양한 딥링크 유형을 알아봤는데요. 이제부터 국내 카드앱·은행앱을 Android, iOS 웹뷰에서 각각 열어볼게요.

### Android 웹뷰에서 딥링크 열기

먼저 `AndroidManifest.xml` 파일에 카드앱·은행앱 [패키지를 등록할게요](https://developer.android.com/training/package-visibility/declaring?hl=ko). 패키지를 등록하지 않으면 앱이 설치되어 있어도 스토어로 이동하고, Google 정책을 위반해요.

```kotlin title="AndroidManifest.xml"
<queries>
  <package android:name="com.kakao.talk" /> <!-- 카카오톡 -->
  <package android:name="com.shcard.smartpay" /> <!-- 신한페이판 -->
  <package android:name="com.shinhancard.smartshinhan" /> <!-- 신한페이판-공동인증서 -->
  <package android:name="com.hyundaicard.appcard" /> <!-- 현대카드 -->
  <package android:name="com.lumensoft.touchenappfree" /> <!-- 현대카드-공동인증서 -->
  <package android:name="kr.co.samsungcard.mpocket" /> <!-- 삼성카드 -->
  <package android:name="nh.smart.nhallonepay" /> <!-- 올원페이 -->
  <package android:name="com.kbcard.cxh.appcard" /> <!-- KB Pay -->
  <package android:name="com.kbstar.liivbank" /> <!-- Liiv(KB국민은행) -->
  <package android:name="com.kbstar.reboot" /> <!-- Liiv Reboot(KB국민은행) -->
  <package android:name="kvp.jjy.MispAndroid320" /> <!-- ISP/페이북 -->
  <package android:name="com.lcacApp" /> <!-- 롯데카드 -->
  <package android:name="com.hanaskcard.paycla" /> <!-- 하나카드 -->
  <package android:name="kr.co.hanamembers.hmscustomer" /> <!-- 하나멤버스 -->
  <package android:name="kr.co.citibank.citimobile" /> <!-- 씨티모바일 -->
  <package android:name="com.wooricard.wpay" /> <!-- 우리페이 -->
  <package android:name="com.wooricard.smartapp" /> <!-- 우리카드 -->
  <package android:name="com.wooribank.smart.npib" /> <!-- 우리WON뱅킹 -->
  <package android:name="viva.republica.toss" /> <!-- 토스뱅크 -->
  <package android:name="com.nhnent.payapp" /> <!-- PAYCO -->
  <package android:name="com.ssg.serviceapp.android.egiftcertificate" /> <!-- SSGPAY -->
  <package android:name="com.kakao.talk" /> <!-- 카카오페이 -->
  <package android:name="com.nhn.android.search" /> <!-- 네이버페이 -->
  <package android:name="com.lotte.lpay" /> <!-- L.pay -->
  <package android:name="com.lottemembers.android" /> <!-- L.POINT -->
  <package android:name="com.samsung.android.spay" /> <!-- 삼성페이 -->
  <package android:name="com.samsung.android.spaylite" /> <!-- 삼성페이 -->
  <package android:name="com.lge.lgpay" /> <!-- 엘지페이 -->
  <package android:name="com.lguplus.paynow" /> <!-- 페이나우 -->
</queries>
```

Android에서 [토스페이먼츠 결제창](/resources/glossary/payment-window)을 [WebViewClient](https://developer.android.com/reference/android/webkit/WebViewClient)로 열어요. WebViewClient를 사용하면 웹뷰에서 일어나는 요청, 상태, 에러를 콜백으로 편리하게 처리할 수 있습니다.

웹뷰에서 결제창을 띄우고 카드를 선택하면 카드앱의 딥링크가 열리죠. 웹뷰에서 URL을 열 때는 [`shouldOverrideUrlLoading`](https://developer.android.com/reference/android/webkit/WebViewClient#shouldOverrideUrlLoading\(android.webkit.WebView,%20android.webkit.WebResourceRequest\))()를 사용해요. 이 메서드는 웹뷰에서 URL을 어떻게 열지 제어해요. 메서드에서 `true`를 반환하면 URL은 개발자가 정의한 대로 열리고 웹뷰에서는 아무것도 하지 않아요. `false`를 반환하면 현재 웹뷰에서 URL을 열어요.

그럼 [`shouldOverrideUrlLoading`](https://developer.android.com/reference/android/webkit/WebViewClient#shouldOverrideUrlLoading\(android.webkit.WebView,%20android.webkit.WebResourceRequest\))() 안에 있는 코드를 살펴볼까요?

먼저 딥링크를 파싱해서 URI 객체로 만들어요. 그리고 스킴을 확인해요.

*   Intent 스킴이라면 `startSchemeIntent()`함수를 호출해요. 코드 하단에 정의된 `startSchemeIntent()` 함수를 보세요. Intent 스킴 링크를 열어보고 앱이 설치 안 되어 있다면, Intent 스킴에 포함된 앱 패키지 정보로 스토어를 열어요.
*   Intent 스킴이 아니라면 커스텀 링크 또는 App Link이겠죠. 해당 딥링크로 Activity를 시작해요. 링크에 오타가 있거나 문제가 있어서 정상적으로 Activity가 실행되지 않는다면, `false`를 반환해서 웹뷰에서 링크를 열어요. 여기에 스토어로 이동하는 로직도 추가할 수도 있어요.

```kotlin
fun initViews() {
	WEB_VIEW.webViewClient = object : WebViewClient() {
      override fun shouldOverrideUrlLoading(
          view: WebView?,
          request: WebResourceRequest?
      ): Boolean {
          return shouldOverrideUrlLoading(view, request)
      }
  }
}

fun shouldOverrideUrlLoading(view: WebView, url: String): Boolean
    url?.let {
        if (!URLUtil.isNetworkUrl(url) && !URLUtil.isJavaScriptUrl(url)) {
			// 딥링크로 URI 객체 만들기
            val uri = try {
                Uri.parse(url)
            } catch (e: Exception) {
                return false
            }

            return when (uri.scheme) {
                "intent" -> {
                    startSchemeIntent(it) // Intent 스킴인 경우
                }
                else -> {
                    return try {
                        startActivity(Intent(Intent.ACTION_VIEW, uri)) // 다른 딥링크 스킴이면 실행
                        true
                    } catch (e: java.lang.Exception) {
                        false
                    }
                }
            }
        } else {
            return false
        }
    } ?: return false

/*Intent 스킴을 처리하는 함수*/
fun startSchemeIntent(url: String): Boolean {
    val schemeIntent: Intent = try {
        Intent.parseUri(url, Intent.URI_INTENT_SCHEME) // Intent 스킴을 파싱
    } catch (e: URISyntaxException) {
        return false
    }
    try {
        startActivity(schemeIntent) // 앱으로 이동
        return true
    } catch (e: ActivityNotFoundException) { // 앱이 설치 안 되어 있는 경우
        val packageName = schemeIntent.getPackage()

        if (!packageName.isNullOrBlank()) {
            startActivity(
                Intent(
                    Intent.ACTION_VIEW,
                    Uri.parse("market://details?id=$packageName") // 스토어로 이동
                )
            )
            return true
        }
    }
    return false
}
```

### iOS 웹뷰에서 딥링크 열기

먼저 `Info.plist` 에 [LSApplicationQueriesSchemes](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/LaunchServicesKeys.html#//apple_ref/doc/uid/TP40009250-SW14) 를 추가하고 카드사, 은행의 앱 스킴을 배열에 넣어 주세요. 설정하지 않으면, 앱이 열리지 않고 콘솔 쪽에 `canOpenURL : failed for URL` 에러가 발생해요.

```swift title="Info.plist"
<key>LSApplicationQueriesSchemes</key>
  <array>
    <string>supertoss</string>
    <string>kb-acp</string>
    <string>liivbank</string>
    <string>newliiv</string>
    <string>kbbank</string>
    <string>nhappcardansimclick</string>
    <string>nhallonepayansimclick</string>
    <string>nonghyupcardansimclick</string>
    <string>lottesmartpay</string>
    <string>lotteappcard</string>
    <string>mpocket.online.ansimclick</string>
    <string>ansimclickscard</string>
    <string>tswansimclick</string>
    <string>ansimclickipcollect</string>
    <string>vguardstart</string>
    <string>samsungpay</string>
    <string>scardcertiapp</string>
    <string>shinhan-sr-ansimclick</string>
    <string>smshinhanansimclick</string>
    <string>com.wooricard.wcard</string>
    <string>newsmartpib</string>
    <string>citispay</string>
    <string>citicardappkr</string>
    <string>citimobileapp</string>
    <string>cloudpay</string>
    <string>hanawalletmembers</string>
    <string>hdcardappcardansimclick</string>
    <string>smhyundaiansimclick</string>
    <string>shinsegaeeasypayment</string>
    <string>payco</string>
    <string>lpayapp</string>
    <string>ispmobile</string>
    <string>tauthlink</string>
    <string>ktauthexternalcall</string>
    <string>upluscorporation</string>
    <string>kftc-bankpay</string>
    <string>kakaotalk</string>
    <string>wooripay</string>
    <string>lmslpay</string>
    <string>naversearchthirdlogin</string>
    <string>hanaskcardmobileportal</string>
    <string>kb-bankpay</string>
  </array>
```

[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) 클래스를 사용해서 [토스페이먼츠 결제창](/resources/glossary/payment-window)을 열게요. 웹뷰에서 다른 URL로 이동하는 것을 제어하려면 [WKNavigationDelgate](https://developer.apple.com/documentation/webkit/wknavigationdelegate)을 사용하세요. WKNavigationDelegate을 conform하고 있는 클래스에서 아래와 같이 `webView()`를 정의하세요.

`webView()`를 더 자세히 볼게요. `decidePolicyFor` 파라미터에 들어가는 [WKNavigationAction](https://developer.apple.com/documentation/webkit/wknavigationaction)을 딥링크로 설정합니다. 함수 안에서 URL 스킴의 유형을 확인할게요.

*   URL 스킴이 `http` 혹은 `https` 이 아니라 커스팀 스킴이면, 링크를 열어봐요. 정상적으로 링크가 열린다면 성공이고 앱이 이동해요. 안 열린다면 사용자 폰에 앱이 설치 안 되어 있어요. 사용자를 수동으로 앱 스토어로 이동시킬 수는 있지만, 이 방법은 유지 보수가 어려워요. 앱 스토어 아이디가 바뀔 수 있기 때문이죠. 2023년 3월 기준으로 국내 카드·은행 앱의 앱 스토어 아이디는 아래 코드에서 확인할 수 있어요.
*   URL 스킴이 `http` 혹은 `https`이라면 Universal Link이겠죠. 웹뷰에서 바로 열면 됩니다. 사용자 폰에 앱이 설치되어 있지 있으면 바로 이동하고, 설치되어 있지 않으면 보통 각 카드·은행 앱에서 사용자를 스토어로 이동시켜요.

```swift
func webView(
    _ webView: WKWebView,
    decidePolicyFor navigationAction: WKNavigationAction,
    decisionHandler: @escaping (WKNavigationActionPolicy) -> Void
) {
    if let url = navigationAction.request.url, // Navigation Action을 딥링크로 설정
    url.scheme != "http" && url.scheme != "https" {
        UIApplication.shared.open(url, options: [:], completionHandler:{ (success) in
            if !(success){
                /* 앱이 설치되어 있지 않을 때
                   카드/은행 앱의 scheme과  앱토어 주소를 알고 있는 경우,
                   직접 앱스토어로 안내한다. */
				if let scheme = url.scheme,
					let appStoreURLString = Constant.appStoreURL[scheme],
					let appStoreURL = URL(string: appStoreURLString) {
						UIApplication.shared.open(appStoreURL)
					} else {
						// 스킴을 실행에 실패했거나, 앱을 안내할 수 없습니다.
                    }
            }
        })
        decisionHandler(.cancel) // 웹뷰에서 링크로 이동하지 않음
    } else {
        decisionHandler(.allow) // 웹뷰에서 링크로 이동
    }
}
```

```swift
// 카드/은행 앱 앱스토어 URL 관리 목록
enum Constant {
  static let appStoreURL: [String: String] = [
    "supertoss': "https://apps.apple.com/app/id839333328",
    "ispmobile': "https://apps.apple.com/app/id369125087",
    "kb-acp': "https://apps.apple.com/app/id695436326",
    "liivbank': "https://apps.apple.com/app/id1126232922",
    "mpocket.online.ansimclick': "https://apps.apple.com/app/id535125356",
    "lottesmartpay': "https://apps.apple.com/app/id668497947",
    "lotteappcard': "https://apps.apple.com/app/id688047200",
    "lpayapp': "https://apps.apple.com/app/id1036098908",
    "lmslpay': "https://apps.apple.com/app/id473250588",
    "cloudpay': "https://apps.apple.com/app/id847268987",
    "hanawalletmembers': "https://apps.apple.com/app/id1038288833",
    "hdcardappcardansimclick': "https://apps.apple.com/app/id702653088",
    "shinhan-sr-ansimclick': "https://apps.apple.com/app/id572462317",
    "wooripay': "https://apps.apple.com/app/id1201113419",
    "com.wooricard.wcard': "https://apps.apple.com/app/id1499598869",
    "newsmartpib': "https://apps.apple.com/app/id1470181651",
    "nhallonepayansimclick': "https://apps.apple.com/app/id1177889176",
    "citimobileapp': "https://apps.apple.com/app/id1179759666",
    "shinsegaeeasypayment': "https://apps.apple.com/app/id666237916",
    "naversearchthirdlogin': "https://apps.apple.com/app/id393499958",
    "payco': "https://apps.apple.com/app/id924292102",
    "kakaotalk': "https://apps.apple.com/app/id362057947",
    "kftc-bankpay': "https://apps.apple.com/app/id398456030"
  ]
}
```

**Writer** 박수연 **Graphic** 이은호, 이나눔

***

📍 **함께 읽으면 좋을 콘텐츠**

*   [토스페이먼츠 웹뷰 가이드](/guides/webview)

*   [딥링크 실전에서 잘 사용하는 방법](/blog/how-to-use-deep-links)
