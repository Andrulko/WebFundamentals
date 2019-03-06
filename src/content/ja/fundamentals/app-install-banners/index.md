project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: アプリのインストール バナーには、ウェブアプリのインストール バナーとネイティブ アプリのインストール バナーの 2 種類があります。バナーを使えば、ブラウザから離れることなく、素早くシームレスにウェブアプリやネイティブアプリをホーム画面に追加することができます。

{# wf_updated_on:2017-09-27 #} {# wf_published_on:2014-12-16 #}

# ウェブアプリのインストール バナー {: .page-title }

{% include "web/_shared/contributors/mattgaunt.html" %} {% include "web/_shared/contributors/paulkinlan.html" %}

アプリのインストール バナーには、**ウェブ**アプリのインストール バナーと[**ネイティブ**](native-app-install) アプリのインストール バナーの 2 種類があります。 バナーを使えば、ブラウザから離れることなく、素早くシームレスにウェブアプリやネイティブアプリをホーム画面に追加できます。

アプリのインストール バナーの追加は簡単で、煩雑な作業はほとんど Chrome が処理してくれます。 アプリの詳細を記述したウェブアプリのマニフェスト ファイルをサイトに追加するだけです。

* On mobile, Chrome will generate a [WebAPK](/web/fundamentals/integration/webapks), creating an even more integrated experience for your users.
* サイトに [Service Worker](/web/fundamentals/getting-started/primers/service-workers) が登録されている。

## アプリのインストール バナー イベント

そうすれば Chrome が一連の条件とアクセス頻度のヒューリスティックに基づいて、バナー表示のタイミングを自動的に判定します。 以降のトピックで、さらに詳しく説明します。

注: ホーム画面への追加（Add to Homescreen、A2HS）は、ウェブアプリのインストール バナーの別名です。この 2 つの用語は同じです。

## Native app install banners

<figure class="attempt-right">
  <img src="images/a2hs-dialog-g.png" alt="Add to Home Screen dialog on Android">
  <figcaption>Add to Home Screen dialog on Android</figcaption>
</figure>

Chrome は、アプリが次の条件を満たすと、自動的にバナーを表示します。

1. Chrome DevTools を開きます。
2. [**Application**] パネルに移動します。
3. [**Manifest**] タブに移動します。

<div class="clearfix"></div>

注: ウェブアプリのインストール バナーは、最先端のテクノロジーなので、アプリのインストール バナーを表示するための条件は、今後変更される可能性があります。ウェブアプリ インストール バナーの最新の条件については、[What, Exactly, Makes Something a Progressive Web App?](https://infrequently.org/2016/09/what-exactly-makes-something-a-progressive-web-app/) をご覧ください。

### 条件について

ウェブアプリ マニフェストをセットアップしたら、正しく定義されているかを検証してください。 必要に応じて 2 つの方法から選択できます。1 つは手動、もう 1 つは自動です。

手動でアプリのインストール バナーをトリガーする方法は次のとおりです。

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
    

### アプリのインストール バナーのテスト{: #test }

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

詳細については、[ホーム画面への追加イベントのシミュレート](/web/tools/chrome-devtools/progressive-web-apps#add-to-homescreen)をご覧ください。

### ユーザーがアプリをインストールしたのかを確認する

アプリのインストール バナーの自動テストでは、Lighthouse を使用します。Lighthouse はウェブアプリの監査ツールで、 Chrome 拡張機能または NPM モジュールとして実行できます。 アプリをテストするには、監査する特定のページを Lighthouse に指定します。 Lighthouse はそのページに対して一連の監査を実行し、ページの結果についてレポートを作成します。

以下のスクリーンショットに示す 2 組の Lighthouse 監査では、アプリのインストール バナーを表示するためにページが合格しなければならないすべてのテストを表しています。

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

Lighthouse の使用を開始するには、[Lighthouse によるウェブアプリの監査](/web/tools/lighthouse/)をご覧ください。

Chrome はユーザーがアプリのインストール バナーにどう反応したのか、キャンセルまたは都合のいいタイミングまで先送りしたかまで特定できる、簡単な仕組みを提供しています。

`beforeinstallprompt` イベントは、ユーザーがプロンプトに応答したときに解決する `userChoice` という Promise を返します。 この Promise は、`outcome` 属性に値 `dismissed` を設定したオブジェクトか、またはウェブページがホーム画面に追加された場合は `accepted` を設定したオブジェクトを返します。

## Feedback {: .hide-from-toc }

この方法は、ユーザーがアプリのインストールを促すプロンプトにどう対応したのかを把握するのに便利です。

<div class="clearfix"></div>

## Determine if the app was successfully installed {: #appinstalled }

Chrome はプロンプトをトリガーするタイミングを管理しますが、一部のサイトではこのタイミングが最適ではない場合があります。 このプロンプトを後でアプリを使用するときまで先送りするか、キャンセルすることができます。

    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

## Detecting if your app is launched from the home screen {: #detect-mode }

### プロンプトの先送りまたはキャンセル

Chrome がユーザーにアプリのインストールを促したときに、デフォルトの操作を阻止して、後でイベントを実行するように保存しておくことができます。 その後、ユーザーがサイトを好意的に操作しているタイミングで、保存したイベントの `prompt()` をもう一度トリガーすることができます。

This causes Chrome to show the banner and all the Promise attributes such as `userChoice` will be available to bind to so that you can understand what action the user took.

    "prefer_related_applications": true,
    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

または、デフォルトの操作を阻止して、プロンプトをキャンセルすることもできます。

    if (window.matchMedia('(display-mode: standalone)').matches) {
      console.log('display-mode is standalone');
    }
    

### バナーの表示条件

ネイティブ アプリのインストール バナーは、[ウェブアプリのインストール バナー](.)と同様ですが、ホーム画面に追加するのではなく、サイトから移動せずにネイティブ アプリをインストールできます。

    if (window.navigator.standalone === true) {
      console.log('display-mode is standalone');
    }
    

## Updating your app's icon and name

### マニフェストの要件

表示条件はウェブアプリのインストール バナーと同様です。ただし、Service Worker が必要になる点が異なります。 サイトの条件:

### Desktop

任意のマニフェストに統合するには、`play`（Google Play 用） のプラットフォームと App ID を指定した `related_applications` 配列を追加します。

## Test your add to home screen experience {: #test }

Android アプリのインストール機能のみをユーザーに提供し、ウェブアプリのインストール バナーを表示する必要がない場合は、`"prefer_related_applications": true` を追加します。

次に例を示します。

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