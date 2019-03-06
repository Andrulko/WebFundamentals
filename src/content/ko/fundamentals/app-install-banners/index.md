project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: 앱 설치 배너는 웹 앱 설치 배너와 네이티브 앱 설치 배너의 두 종류가 있습니다. 앱 설치 배너를 통해 사용자는 브라우저를 떠나지 않고서도 자신의 홈 화면에 여러분의 웹 앱이나 네이티브 앱을 빠르고 정확하게 추가할 수 있습니다.

{# wf_updated_on: 2017-09-27 #} {# wf_published_on: 2014-12-16 #}

# 웹 앱 설치 배너 {: .page-title }

{% include "web/_shared/contributors/mattgaunt.html" %} {% include "web/_shared/contributors/paulkinlan.html" %}

앱 설치 배너는 **웹** 앱 설치 배너와 [**네이티브**](native-app-install) 앱 설치 배너의 두 종류가 있습니다. 앱 설치 배너를 통해 사용자는 브라우저를 떠나지 않고서도 자신의 홈 화면에 여러분의 웹 앱이나 네이티브 앱을 빠르고 정확하게 추가할 수 있습니다.

앱 설치 배너는 추가하기가 쉬우며, Chrome이 대부분의 힘든 작업을 대신 처리해 줍니다. 앱에 대한 상세정보와 함께 웹 앱 매니페스트 파일을 여러분의 사이트에 포함해야 합니다.

* On mobile, Chrome will generate a [WebAPK](/web/fundamentals/integration/webapks), creating an even more integrated experience for your users.
* [서비스 워커](/web/fundamentals/getting-started/primers/service-workers)가 여러분 사이트에 등록됨.

## 앱 설치 배너 이벤트

그러면 Chrome은 일련의 기준과 방문 빈도 추론을 사용하여 언제 배너를 표시할지 판별합니다. 자세한 내용을 더 읽어보세요.

참고: Add to Homescreen(약자: A2HS)은 웹 앱 설치 배너를 일컫는 또 다른 이름입니다. 이 두 용어는 같은 뜻입니다.

## 네이티브 앱 설치 배너

<figure class="attempt-right">
  <img src="images/a2hs-dialog-g.png" alt="Add to Home Screen dialog on Android">
  <figcaption>Add to Home Screen dialog on Android</figcaption>
</figure>

앱이 다음 기준을 충족하면 Chrome이 배너를 자동으로 표시합니다.

1. Chrome DevTools를 엽니다.
2. **Application** 창으로 이동합니다.
3. **Manifest** 탭으로 이동합니다.

<div class="clearfix"></div>

참고: 웹 앱 설치 배너는 새로운 기술입니다. 앱 설치 배너를 표시하는 기준은 향후 변경될 수 있습니다. 최신 웹 앱 설치 배너 기준에 대한 자세한 내용은 [What, Exactly, Makes Something a Progressive Web App?](https://infrequently.org/2016/09/what-exactly-makes-something-a-progressive-web-app/)을 참조하세요(추후 추가 업데이트).

### 어떤 기준이 있습니까?

웹 앱 매니페스트 설정이 완료되면 올바르게 정의되었는지 검증하고 싶을 것입니다. 사용할 수 있는 방법은 두 가지입니다. 하나는 수동이고 다른 하나는 자동입니다.

앱 설치 배너를 수동으로 트리거하려면:

    window.addEventListener('beforeinstallprompt', function(e) {
      // beforeinstallprompt Event fired
    
      // e.userChoice will return a Promise. 
      // For more details read: https://developers.google.com/web/fundamentals/getting-started/primers/promises
      e.userChoice.then(function(choiceResult) {
    
        console.log(choiceResult.outcome);
    
        if(choiceResult.outcome == 'dismissed') {
          console.log('User cancelled home screen install');
        }
        else {
          console.log('User added to home screen');
        }
      });
    });
    

### 앱 설치 배너 테스트 {: #test }

The best way to notify the user your app can be installed is by adding a button or other element to your user interface. **Don't show a full page interstitial or other elements that may be annoying or distracting.**

<pre class="prettyprint">window.addEventListener('beforeinstallprompt', (e) => {
  // Prevent Chrome 67 and earlier from automatically showing the prompt
  e.preventDefault();
  // Stash the event so it can be triggered later.
  deferredPrompt = e;
  <strong>// Update UI notify the user they can add to home screen
  btnAdd.style.display = 'block';</strong>
});
</pre>

자세한 도움말은 [Add to Homescreen 이벤트 시뮬레이션](/web/tools/chrome-devtools/progressive-web-apps#add-to-homescreen)을 참조하세요.

### 사용자가 앱을 설치했나요?

앱 설치 배너를 자동 테스트할 때는 Lighthouse를 사용합니다. Lighthouse는 웹 앱 감사 도구입니다. Chrome 확장 프로그램이나 NPM 모듈로 실행할 수 있습니다. 앱을 테스트하려면 Lighthouse에 감사할 페이지를 입력합니다. Lighthouse는 해당 페이지에 대해 감사 스위트를 실행하고 이 페이지의 감사 결과를 보고서로 제공합니다.

아래 스크린샷에서 2개의 Lighthouse 감사 스위트는 페이지에 앱 설치 배너를 표시하기 위해 통과해야 하는 모든 테스트를 의미합니다.

    window.addEventListener('beforeinstallprompt', function(e) {
      console.log('beforeinstallprompt Event fired');
      e.preventDefault();
      return false;
    });
    

You can only call `prompt()` on the deferred event once. If the user dismisses it, you'll need to wait until the `beforeinstallprompt` event is fired on the next page navigation.

## The mini-info bar

<figure class="attempt-right">
  <img
      class="screenshot"
      src="/web/updates/images/2018/06/a2hs-infobar-cropped.png">
  <figcaption>
    The mini-infobar
  </figcaption>
</figure>

Lighthouse를 시작하려면 [Lighthouse로 웹 앱 감사](/web/tools/lighthouse/)를 참조하세요.

Chrome에서 여러분은 사용자가 앱 설치 배너에 반응하는 방식을 쉽게 이해할 수 있고, 더욱 편리한 시간이 될 때까지 앱 설치 배너를 연기하거나 취소할 수 있습니다.

`beforeinstallprompt` 이벤트는 사용자가 프롬프트에서 작업을 수행할 때 분석되는 `userChoice`라고 불리는 프라미스(promise)를 반환합니다. 프라미스는 `outcome` 속성에서 `dismissed` 값을 가진 객체를 반환하거나, 사용자가 웹페이지를 홈 화면에 추가한 경우 `accepted`를 반환합니다.

## Feedback {: .hide-from-toc }

이는 사용자가 앱 설치 프롬프트와 어떻게 상호작용하는지 이해하기 위한 좋은 방법입니다.

<div class="clearfix"></div>

## Determine if the app was successfully installed {: #appinstalled }

Chrome은 프롬프트를 트리거하는 시기를 관리하지만 일부 사이트에는 이것이 바람직하지 않을 수 있습니다. 앱 사용 시에 프롬프트를 나중으로 연기할 수 있으며, 심지어 취소할 수도 있습니다.

    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

## Detecting if your app is launched from the home screen {: #detect-mode }

### 프롬프트 연기 또는 취소

Chrome이 사용자에게 앱을 설치하라는 프롬프트를 표시하는 경우, 여러분이 기본 동작을 차단하고 나중을 위해 이벤트를 저장할 수 있습니다. 그런 다음, 사용자가 여러분의 사이트와 긍정적인 상호작용을 한다면, 저장된 이벤트에서 `prompt()`를 호출하여 프롬프트를 다시 트리거할 수 있습니다.

이 경우 Chrome이 배너를 표시하며, `userChoice`와 같은 모든 Promise 속성을 바인딩에 사용할 수 있으므로, 사용자가 어떤 동작을 취했는지 알 수 있습니다.

    "prefer_related_applications": true,
    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

또는, 기본값을 차단하여 프롬프트를 취소할 수 있습니다.

    if (window.matchMedia('(display-mode: standalone)').matches) {
      console.log('display-mode is standalone');
    }
    

### 배너 표시 기준

네이티브 앱 설치 배너는 [웹 앱 설치 배너](.)와 유사하지만 홈 화면에 추가하는 대신 사용자가 사이트를 떠나지 않고 네이티브 앱을 설치합니다.

    if (window.navigator.standalone === true) {
      console.log('display-mode is standalone');
    }
    

## Updating your app's icon and name

### 매니페스트 요구사항

이 기준은 서비스 워커가 필요하다는 점을 제외하고는 웹 앱 설치 배너와 유사합니다. 사이트는 다음을 충족해야 합니다.

### Desktop

매니페스트에 통합하려면 `play` 플랫폼(Google Play용)과 앱 ID가 있는 `related_applications` 배열을 추가합니다.

## Test your add to home screen experience {: #test }

사용자에게 Android 애플리케이션 설치 기능만 제공하고 웹 앱 설치 배너를 표시하지 않으려면 `"prefer_related_applications": true`를 추가합니다. 예를 들면 다음과 같습니다.

{# wf_devsite_translation #}

### Chrome for Android

1. Open a [remote debugging](/web/tools/chrome-devtools/remote-debugging/) session to your phone or tablet.
2. Go to the **Application** panel.
3. Go to the **Manifest** tab.
4. Click **Add to home screen**

### Chrome OS, Linux, or Windows

1. Open Chrome DevTools
2. Go to the **Application** panel.
3. Go to the **Manifest** tab.
4. Click **Add to home screen**

Dogfood: To test the install flow for Desktop Progressive Web Apps on Mac, you'll need to enable the `#enable-desktop-pwas` flag.

### Will `beforeinstallprompt` be fired?

The easiest way to test if the `beforeinstallprompt` event will be fired, is to use [Lighthouse](/web/tools/lighthouse/) to audit your app, and check the results of the [User Can Be Prompted To Install The Web App](/web/tools/lighthouse/audits/install-prompt) test.