project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description:应用安装横幅有两种：网络应用安装横幅和本机应用安装横幅。这两种应用安装横幅让您的用户可以快速无缝地将您的网络或本机应用添加到他们的主屏幕，无需退出浏览器。

{# wf_updated_on:2017-09-27 #} {# wf_published_on:2014-12-16 #}

# 网络应用安装横幅 {: .page-title }

{% include "web/_shared/contributors/mattgaunt.html" %} {% include "web/_shared/contributors/paulkinlan.html" %}

应用安装横幅有两种：**网络**应用安装横幅和[**本机**](native-app-install)应用安装横幅。 这两种应用安装横幅让您的用户可以快速无缝地将您的网络或本机应用添加到他们的主屏幕，无需退出浏览器。

添加应用安装横幅很轻松，Chrome 会为您处理大部分的繁重工作。 您需要在您的网站中添加一个包含您的应用详细信息的网络应用清单文件。

* On mobile, Chrome will generate a [WebAPK](/web/fundamentals/integration/webapks), creating an even more integrated experience for your users.
* 拥有一个在您的网站上注册的[服务工作线程](/web/fundamentals/getting-started/primers/service-workers)。

## 应用安装横幅事件

然后，Chrome 使用一组条件和访问频率启发式算法来确定何时显示横幅。 请继续阅读以了解更多详情。

Note: Add to Homescreen（有时缩写为 A2HS）是网络应用安装横幅的另一个名称。两个术语相等同。

## Native app install banners

<figure class="attempt-right">
  <img src="images/a2hs-dialog-g.png" alt="Add to Home Screen dialog on Android">
  <figcaption>Add to Home Screen dialog on Android</figcaption>
</figure>

Chrome 将在您的应用符合以下条件时自动显示横幅：

1. 打开 Chrome DevTools。
2. 转到 **Application** 面板。
3. 转到 **Manifest** 标签。

<div class="clearfix"></div>

Note: 网络应用安装横幅是一种新兴技术。显示应用安装横幅的条件将来可能会有所变化。请参阅[究竟是什么造就了 Progressive Web App？](https://infrequently.org/2016/09/what-exactly-makes-something-a-progressive-web-app/)，了解最新网络应用安装横幅条件中的规范引用（将随时间推移不断更新）。

### 条件有哪些？

设置网络应用清单后，您会想要验证它是否已正确定义。 有两种方法供您选择。一种是手动，另一种是自动。

要手动触发应用安装横幅，请执行以下操作：

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
    

### 测试应用安装横幅 {: #test }

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

请参阅[模拟“Add to Homescreen”事件](/web/tools/chrome-devtools/progressive-web-apps#add-to-homescreen)，获取更多帮助。

### 用户是否安装了此应用？

要实现应用安装横幅的自动化测试，请使用 Lighthouse。Lighthouse 是一个网络应用审核工具。 您可以将其作为 Chrome 扩展程序或 NPM 模块运行。 要测试您的应用，您需要为 Lighthouse 提供要审核的特定页面。 Lighthouse 会对此页面运行一套审核，然后以报告形式显示结果。

下面屏幕截图中的两套 Lighthouse 审核显示了您的页面需要通过才能显示应用安装横幅的所有测试。

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

请参阅[使用 Lighthouse 审查网络应用](/web/tools/lighthouse/)，开始使用 Lighthouse。

Chrome 提供一个简单的机制，用于确定用户如何响应应用安装横幅，甚至可以取消或延迟应用安装横幅以等待一个更方便的时间。

`beforeinstallprompt` 事件返回一个名为 `userChoice` 的 promise，并当用户对提示进行操作时进行解析。 promise 会对 `outcome` 属性返回一个值为 `dismissed` 或 `accepted` 的对象，如果用户将网页添加到主屏幕，则返回后者。

## Feedback {: .hide-from-toc }

利用此工具，可以很好地了解您的用户如何与应用安装提示进行互动。

<div class="clearfix"></div>

## Determine if the app was successfully installed {: #appinstalled }

Chrome 可管理触发提示的时间，但对于部分网站而言，这可能不是理想的做法。 您可以在应用使用中延迟触发提示的时间，或甚至取消它。

    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

## Detecting if your app is launched from the home screen {: #detect-mode }

### 延迟或取消提示

当 Chrome 决定提示用户安装应用时，您可以阻止默认操作，并存储此事件以便稍后使用。 然后，当用户与您的网站进行积极互动时，您可以通过对存储的事件调用 `prompt()` 重新触发提示。

这将使 Chrome 显示横幅和所有 Promise 属性，例如，您可绑定到 `userChoice`，以便您可以了解用户进行的操作。 var deferredPrompt; window.addEventListener('beforeinstallprompt', function(e) {

    "prefer_related_applications": true,
    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

或者，您可以通过阻止默认值取消提示框。

    if (window.matchMedia('(display-mode: standalone)').matches) {
      console.log('display-mode is standalone');
    }
    

### 显示横幅的条件

本机应用安装横幅类似于[网络应用安装横幅](.)，它们可以让用户无需离开网站即可安装您的本机应用，而不用将应用添加到主屏幕。

    if (window.navigator.standalone === true) {
      console.log('display-mode is standalone');
    }
    

## Updating your app's icon and name

### 清单要求

除了需要服务工作线程外，条件类似于网络应用安装横幅。 您的网站必须满足以下条件：

### Desktop

要集成到任何清单中，请添加一个包含 `play` 平台（针对 Google Play）和应用 ID 的 `related_applications` 数组。

## Test your add to home screen experience {: #test }

如果只是想要用户可以安装您的 Android 应用，而不显示网络应用安装横幅，那么请添加 `"prefer_related_applications": true`。

例如：

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

{# wf_devsite_translation #}

### Will `beforeinstallprompt` be fired?

The easiest way to test if the `beforeinstallprompt` event will be fired, is to use [Lighthouse](/web/tools/lighthouse/) to audit your app, and check the results of the [User Can Be Prompted To Install The Web App](/web/tools/lighthouse/audits/install-prompt) test.