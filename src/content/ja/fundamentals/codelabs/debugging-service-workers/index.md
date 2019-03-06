project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: このコードラボでは、新しい DevTools の [Application] パネルを使用して、Service Worker をデバッグする方法について説明します。また、プッシュ通知をシミュレートして登録が正常にセットアップされていることを確認する方法についても説明します。

{# wf_auto_generated #} {# wf_updated_on:2016-10-19 #} {# wf_published_on:2016-01-01 #}

# Service Worker のデバッグ {: .page-title }

{% include "web/_shared/contributors/robdodson.html" %}

## はじめに

Service Worker を使用すると、デベロッパーはムラのあるネットワークに対処したり、オフラインファーストのウェブアプリを作成したりできます。しかし、新しいテクノロジーであるということは、場合によってはデバッグが難しいということも意味します。特に、ツールが追いつくまで待つことになるからです。

このコードラボでは、基本的な Service Worker の作成を詳細に説明し、Chrome DevTools の新しい [Application] パネルを使用してワーカーのデバッグや調査を行う方法を紹介しています。

### 作成するもの

![6ffdd0864a80600.png](img/6ffdd0864a80600.png)

このコードラボでは、非常に単純なプログレッシブ ウェブアプリを使用して、問題が発生した際にアプリで利用できる手法について説明します。

このコードラボはツールの説明に重点を置いているため、さまざまなポイントや実験に自由に時間を割いてください。コードを使用したり、ページを更新したり、新しいタブを開いたりしてみてください。デバッグツールの学習に最適な方法は、自分で何かを壊してそれを修正してみることです。

### 学習内容

* [Application] パネルを使用して Service Worker を調べる方法
* キャッシュと IndexedDB を調べる方法
* 異なるネットワーク条件をシミュレートする方法
* デバッガー文とブレークポイントを使用して Service Worker をデバッグする方法
* プッシュ イベントをシミュレートする方法

### 必要なもの

* Chrome 52 以上
* [Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb) をインストールするか、ご利用のウェブサーバーを使用します。
* サンプルコード
* テキスト エディタ
* HTML、CSS、JavaScript の基本知識

このコードラボは、Service Worker のデバッグに重点を置いているため、Service Worker を使用するいくつかの予備知識が必要となります。いくつかの概念については説明を省略しています。また、単にコピーして貼り付けられるコードブロック（スタイルや関連性のない JavaScript）を用意している場合もあります。初めて Service Worker を使用する場合は、先に進む前に [API 入門の内容をご確認ください](/web/fundamentals/primers/service-worker/?hl=en)。

## セットアップ

### コードのダウンロード

このコードラボのすべてのコードをダウンロードするには、次のボタンをクリックします。

[リンク](https://github.com/googlecodelabs/debugging-service-workers/archive/master.zip)

ダウンロードした zip ファイルを解凍します。これにより、ルートフォルダ（`debugging-service-workers-master`）が解凍されます。これには、このコードラボのステップごとに 1 つのフォルダと必要なすべてのリソースが含まれます。

`step-NN` フォルダには、このコードラボの各ステップの目標となる最終状態が含まれています。これは参照用に用意されています。すべてのコーディング作業は `work` というディレクトリで行います。

### ウェブサーバーのインストールと確認

自分のウェブサーバーを使用することができますが、このコードラボは、Chrome Web Server で問題なく動作するように設計されています。このアプリをまだインストールしていない場合は、Chrome ウェブストアからインストールできます。

[リンク](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb)

Web Server for Chrome アプリをインストールしたら、ブックマーク バーの [Apps] ショートカットをクリックします。

![9efdf0d1258b78e4.png](img/9efdf0d1258b78e4.png)<aside class="key-point">

<p>More help:  <a href="https://support.google.com/chrome_webstore/answer/3060053?hl=en">Add and open Chrome apps</a></p>

</aside> 

次のウィンドウで、ウェブサーバー アイコンをクリックします。

![dc07bbc9fcfe7c5b.png](img/dc07bbc9fcfe7c5b.png)

次にこのダイアログが表示され、ローカル ウェブサーバーを構成できます。

![433870360ad308d4.png](img/433870360ad308d4.png)

[**choose folder**] ボタンをクリックし、`work` フォルダを選択します。これにより、ウェブサーバー ダイアログ（[**Web Server URL(s)**] セクション）でハイライト表示された URL から進行中の作業を表示できます。

オプションで、以下に示すように [Automatically show index.html] の横のボックスをオンにします。

![8937a38abc57e3.png](img/8937a38abc57e3.png)

次に、[Web Server: STARTED] というトグルを左右にスライドしてサーバーを停止したり再開したりします。

![daefd30e8a290df5.png](img/daefd30e8a290df5.png)

ここで、ご利用のウェブブラウザで（ハイライト表示されたウェブサーバー URL をクリックして）作業サイトにアクセスすると、次のようなページが表示されます。

![693305d127d9fe80.png](img/693305d127d9fe80.png)

明らかにこのアプリに変わったところはありません。以降のステップで、オフラインで動作することを確認できるように機能を追加します。<aside class="key-point">

<p>From this point forward, all testing/verification should be performed using this web server setup. You'll usually be able to get away with simply refreshing your test browser tab.</p>

</aside> 

## [Application] タブの紹介

### マニフェストの調査

Progressive Web App の構築には、中核をなすさまざまなテクノロジー（Service Worker やウェブアプリ マニフェストなど）をひとまとめにし、Cache Storage API、IndexedDB、プッシュ通知などのテクノロジーを有効化する必要があります。デベロッパーがこれらの各テクノロジーの調整されたビューを簡単に取得できるように、Chrome DevTools では新しい [Application] パネルのそれぞれにインスペクターを組み込んでいます。

* Chrome DevTools を開いて [**Application**] というタブをクリックします。

![b380532368b4f56c.png](img/5d18df60c53a0420.png)

サイドバーを見ると、現在 [**Manifest**] がハイライト表示されていることがわかります。このビューには、`manifest.json` ファイルに関連する重要な情報（そのアプリ名、起動時の URL、アイコンなど）が表示されます。

このコードラボでは説明しませんが、[**Add to homescreen**] ボタンがあり、これを使用してユーザーのホーム画面にアプリを追加する操作性をシミュレートできます。

![56508495a6cb6d8d.png](img/56508495a6cb6d8d.png)

### Service Worker の調査

以前は、Service Worker を調査するには Chrome の内部を調べる必要があり、まったくユーザー フレンドリーな操作ではありませんでした。新しい [**Application**] タブにより、このすべてが変わりました。

* 現在選択されている [**Manifest**] アイテムの下の [**Service Workers**] メニュー アイテムをクリックします。

![3dea544e6b44979d.png](img/d4eeba0a3c66a04.png)

[**Service Workers**] ビューに、現在のオリジンでアクティブな Service Worker に関する情報が表示されます。一番上の行に一連のチェックボックスが表示されます。

* [**Offline**] - ネットワークから切断されていることをシミュレートします。これは、Service Worker の fetch ハンドラが正常に動作していることを簡単に確認するのに役立ちます。
* [**Update on reload**] - 現在の Service Worker を新しい Service Worker で置換します（デベロッパーが `service-worker.js` へのアップデートを行った場合）。通常、ブラウザは、新しい Service Worker にアップデートする前に現在のサイトを含むすべてのタブをユーザーが閉じるまで待機します。
* [**Bypass for network**] - ブラウザがすべてのアクティブな Service Worker を無視してネットワークからリソースをフェッチするようにします。これは、CSS や JavaScript を使用し、Service Worker が誤って古いファイルをキャッシュしたり返したりする心配がない場合に非常に役に立ちます。
* [**Show all**] - オリジンに関係なくアクティブなすべての Service Worker のリストを表示します。

その下は現在のアクティブな Service Worker に関連する情報が表示されます（1 つの場合）。最も役立つフィールドの 1 つは [**Status**] フィールドで、これには Service Worker の現在の状態が表示されます。アプリを起動したのは今回が初めてのため、現在の Service Worker は正常にインストールされ、アクティベートされています。そのため、問題がないことを示す緑色の円が表示されています。<aside class="key-point">

<p>If you had installed a service worker on this localhost port previously, you will see an orange circle as well, indicating that the new service worker is waiting to activate. If this is the case, click <strong>skipWaiting</strong>.</p>

</aside> 

緑色のステータス インジケーターの横の ID 番号に注目してください。これは現在アクティブな Service Worker の ID です。すぐにこれを使用して比較するため、覚えておくか、書き留めてください。

* テキスト エディタで `service-worker.js` ファイルを開きます。

現在の Service Worker のコードは非常に簡単で、数個のコンソール ログだけです。

    self.addEventListener('install', function(event) {
      console.log('Service Worker installing.');
    });
    
    self.addEventListener('activate', function(event) {
      console.log('Service Worker activating.');  
    });
    

DevTools に切り替えて [Console] を見ると、どちらのログも正常に出力されていることがわかります。

![5fcfd389f5357c09.png](img/a8c1d1bb2a14eb24.png)

`service-worker.js` のコードをアップデートして、ライフサイクルの変化を観察してみてください。

* 新しいメッセージが含まれているため、`service-worker.js` のコメントをアップデートします。
    
    self.addEventListener('install', function(event) { console.log('A *new* Service Worker is installing.'); });
    
    self.addEventListener('activate', function(event) { console.log('Finally active. Ready to start serving content!');  
    });

* ページを更新して DevTools のコンソールを開きます。

`A *new* Service Worker is installing.` というコンソール ログは表示されていますが、アクティブな新しい Service Worker に関する 2 番目のメッセージは表示されていません。

* DevTools の [Application] タブに切り替えます。

[Application] タブに 2 つのステータス インジケーターが表示されています。これらは 2 つの Service Worker の状態を示しています。

![2e41dbf21437944c.png](img/67548710d5ca4936.png)

最初の Service Worker の ID に注目してください。これは本来の Service Worker ID と一致する必要があります。新しい Service Worker をインストールした場合は、次回ユーザーがページにアクセスするまで前のワーカーはアクティブなままです。

2 番目のステータス インジケーターは、編集したばかりの新しい Service Worker を示しています。現時点ではこれは待機状態です。<aside class="key-point">

<p><strong>Try it!</strong></p>

<p>If a user has multiple tabs open for the same page, it will continue using the old Service Worker until those tabs are closed. Try opening a few more tabs and visiting this same page and notice how the Application panel still shows the old Service Worker as active.</p>

</aside> 

新しい Service Worker をアクティベートする簡単な方法は、[**skipWaiting**] ボタンを使用することです。

![7a60e9ceb2db0ad2.png](img/7a60e9ceb2db0ad2.png)

* [skipWaiting] ボタンをクリックして [Console] に切り替えます。

コンソールに、`activate` イベント ハンドラからメッセージが出力されていることに注意してください。

`Finally active. Ready to start serving content!`<aside class="key-point">

<p><strong>Skip waiting</strong></p>

<p>Having to click the <code>skipWaiting</code> button all the time can get a little annoying. If you'd like your Service Worker to force itself to become active you can include the line <code>self.skipWaiting()</code> in the <code>install</code> event handler. You can learn more about the <code>skipWaiting</code> method in  <a href="https://slightlyoff.github.io/ServiceWorker/spec/service_worker/index.html#service-worker-global-scope-skipwaiting">the Service Workers spec</a>.</p>

</aside> 

## キャッシュの調査

Service Worker は、ユーザーのオフライン ファイルのキャッシュの管理に非常に有用です。新しい [**Application**] パネルには、開発時に非常に役に立つ、格納されているリソースを調査したり変更したりするのに便利ないくつかのツールが用意されています。

### Service Worker へのキャッシュの追加

キャッシュを調査する前に、コードを少し記述していくつかのファイルを格納する必要があります。Service Worker のインストール段階でのファイルの事前キャッシュは、重要なリソースがオフラインになった場合にユーザーが利用できるようにする便利な手法です。では始めましょう。

* `service-worker.js` をアップデートする前に、DevTools の [**Application**] パネルを開いて [**Service Workers**] メニューに移動し、[**Update on reload**] というボックスをオンにします

![d4bcfb0983246797.png](img/d4bcfb0983246797.png)

この便利なトリックにより、Service Worker が最新でもページで使用できるため、Service Worker を変更するたびに [**skipWaiting**] オプションをクリックする必要はありません。

* 次に、以下に示すように `service-worker.js` のコードをアップデートします。

    var CACHE_NAME = 'my-site-cache-v1';
    var urlsToCache = [
      '/',
      '/styles/main.css',
      '/scripts/main.js',
      '/images/smiley.svg'
    ];
    
    self.addEventListener('install', function(event) {
      // Perform install steps
      event.waitUntil(
        caches.open(CACHE_NAME)
          .then(function(cache) {
            return cache.addAll(urlsToCache);
          })
      );  
    });
    
    self.addEventListener('activate', function(event) {
      console.log('Finally active. Ready to start serving content!');  
    });
    

* ページを更新します。

[Application] パネルに [Error] が表示されていることがわかります。これには驚きますが、[**details**] ボタンをクリックすると、古い Service Worker が強制的にアップデートされたことを [**Application**] パネルが示していることがわかります。これは意図的だったため全体として問題ありませんが、便利な警告として機能するため、`service-worker.js` ファイルの編集を実行しているときにチェックボックスをオフにすることを忘れることがありません。

![a039ca69d2179199.png](img/c6363ac5b51e06b1.png)

### キャッシュ ストレージの調査

[**Application**] パネルの [**Cache Storage**] メニュー アイテムには、展開できることを示すキャレットが表示されていることに注目してください。

* クリックして [**Cache Storage**] メニューを展開し、`my-site-cache-v1` をクリックします。

![af2b3981c63b1529.png](img/7990023bd9e8fe7a.png)

ここで、Service Worker によってキャッシュされたすべてのファイルを確認できます。キャッシュからファイルを削除する必要がある場合は、そのファイルを右クリックして、コンテキスト メニューから [**delete**] オプションを選択できます。同様に、キャッシュ全体を削除するには、`my-site-cache-v1` を右クリックして [delete] を選択します。

![5c8fb8f7948066e6.png](img/5c8fb8f7948066e6.png)

### スレートの消去

[**Cache Storage**] の他に、次のような格納されたリソースに関連するいくつかのメニュー アイテムがあります。[Local Storage]、[Session Storage]、[IndexedDB]、[Web SQL]、[Cookies]、[Application Cache]（AppCache）。1 つのパネルで、これらの各リソースを細かく制御できるのは非常に便利です。しかし、格納されたすべてのリソースを削除する場合は、各メニュー アイテムにアクセスして、これらのコンテンツを削除する必要があり、かなり面倒です。代わりに、[**Clear storage**] オプションを使用してスレートを一度に消去することができます（これはすべての Service Worker の登録を解除することにも注意してください）。

* [**Clear storage**] メニュー オプションを選択します。
* [**Clear site data**] ボタンをクリックして、格納されたすべてのリソースを削除します。

![59838a73a2ea2aaa.png](img/744eb12fec050d31.png)

戻って `my-site-cache-v1` をクリックすると、格納されたすべてのファイルが削除されていることがわかります。

![317d24238f05e69c.png](img/3d8552f02b82f4d5.png)<aside class="key-point">

<p><strong>TIP:</strong> You can also use a new Incognito window for testing and debugging Service Workers. When the Incognito window is closed, Chrome will remove any cached data or installed Service Worker, ensuring that you always start from a clean state.</p>

</aside> 

**歯車の意味**

Service Worker はその独自のネットワーク リクエストを生成できるため、ワーカー自体が発生元のネットワークトラフィックを識別するのに役立ちます。

* `my-site-cache-v1` が空のうちに、[Network] パネルに切り替えます。
* ページを更新します。

[Network] パネルには、`main.css` などのファイルのリクエストの初期セット、同じアセットをフェッチしたと思われる、先頭に歯車アイコンが付いた 2 回目のリクエストが表示されます。

![2ba393cf3d41e087.png](img/8daca914fe2d9dc7.png)

歯車アイコンは、これらのリクエストが Service Worker 自体から送信されていることを示しています。特に、これらはオフライン キャッシュに移入するために Service Worker の `install` ハンドラによって生成されたリクエストです。<aside class="key-point">

<p><strong>Learn More</strong>: For a deeper understanding of the Network panel identifies Service Worker traffic take a look at  <a href="http://stackoverflow.com/a/33655173/385997">this StackOverflow discussion</a>.</p>

</aside> 

## 異なるネットワーク条件のシミュレート

Service Worker の素晴らしい機能の 1 つは、ユーザーがオフラインでもキャッシュされたコンテンツを提供できることです。すべてが計画どおりに機能していることを確認するには、Chrome が提供するネットワーク スロットリング ツールの一部をテストしてみましょう。

### オフライン中のリクエストの処理

オフライン コンテンツを処理するには、`service-worker.js` に `fetch` ハンドラを追加する必要があります

* `activate` ハンドラの直後の `service-worker.js` に次のコードを追加します。

    self.addEventListener('fetch', function(event) {
      event.respondWith(
        caches.match(event.request)
          .then(function(response) {
            // Cache hit - return response
            if (response) {
              return response;
            }
            return fetch(event.request);
          }
        )
      );
    });
    

* [**Application**] パネルに切り替えて、[**Update on reload**] がオンになっていることを確認します。
* ページを更新して、新しい Service Worker をインストールします。
* [**Update on reload**] をオフにします。
* [**Offline**] をオンにします。

[**Application**] パネルは次のようになります。

![873b58278064b627.png](img/54d7f786f2a8838e.png)

[**Network**] パネルに、オフラインであること（およびネットワークを使用して開発を続行する場合にそのチェックボックスをオフにする必要があること）を示す黄色の警告サインが表示されていることに注目してください。

`fetch` ハンドラはそのままにして、アプリを [**Offline**] に設定します。これが重要です。ページを更新します。うまくいった場合はネットワークから何も送信されていなくても、サイト コンテンツが続けて表示されます。[**Network**] パネルに切り替えて、リソースのすべてがキャッシュ ストレージから提供されていることを確認できます。[**Size**] 列に、これらのリソースが `(from Service Worker)` から取得されていることが示されています。これは、ネットワークに接続せず、Service Worker がリクエストをインターセプトして、キャッシュからレスポンスを提供したことを示すシグナルです。

![a6f485875ca088db.png](img/96f2065b2f0adece.png)

失敗したリクエストがあることがわかります（新しい Service Worker または `manifest.json` 用など）。これは全体として問題がなく、想定どおりです。

### 低速ネットワークや不安定なネットワークのテスト

多量のさまざまなコンテキストでモバイル端末を使用するため、さまざまな接続状態の中を絶えず移動しています。さらに世界には 3G や 2G の速度が一般的な場所も多数あります。これらのユーザーにアプリがうまく動作していることを確認するには、低速接続でも機能することをテストする必要があります。

まず、Service Worker が動作していない場合に、低速ネットワークでアプリがどのように動作するかをシミュレートしましょう。

* [**Application**] パネルで [**Offline**] をオフにします。
* [**Bypass for network**] をオンにします。

![739dc5811e4aa937.png](img/d9ea0e24a6ef374e.png)

[**Bypass for network**] オプションは、ブラウザにネットワーク リクエストの生成が必要な場合は、Service Worker をスキップするよう指示します。これは、キャッシュ ストレージから何も取得できず、Service Worker がインストールされていない状態と同じであることを意味します。

* 次に、[**Network**] パネルに切り替えます。
* [**Network Throttle**] ドロップダウンを使用して、ネットワーク速度を `Regular 2G` に設定します。

[**Network Throttle**] ドロップダウンは、[**Network**] パネルの右上にあり、[**Network**] パネルの [**Offline**] チェックボックスのすぐ横にあります。デフォルトでは、`No throttling` に設定されています。

![c59b54a853215598.png](img/662a15b44afb6633.png)

* 速度を `Regular 2G` に設定してページを更新します。

レスポンス タイムが向上していることがわかります。現在、各アセットのダウンロードに数百ミリ秒かかっています。

![70e461338a0bb051.png](img/9774a4c588a6604c.png)

Service Worker を動作させるとどのような違いがあるかを確認しましょう。

* ネットワークを `Regular 2G` に設定したまま、[**Application**] タブに切り替えます。
* [**Bypass for network**] チェックボックスをオフにします。
* [**Network**] パネルに切り替えます。
* ページを更新します。

レスポンス タイムがリソースあたり数ミリ秒と高速になっています。低速ネットワークのユーザーにとって、これはまったく違います。

![f0f6d3b0a1b1f18d.png](img/44253f3de0e694b8.png)<aside class="warning">

<p>Before proceeding make sure you set the <strong>Network Throttle</strong> back to <code>No throttling</code></p>

</aside> 

## JavaScript であることに留意

Service Worker は魔法のように感じられますが、中身は単に通常の JavaScript ファイルです。つまり、これらは、`debugger` 文やブレークポイントなどの既存のツールを使用してデバッグできます。

### デバッガーの使用

多くのデベロッパーはアプリに問題が発生した場合は、古い優れた `console.log()` を利用します。しかし、ツールボックスには利用できるより強力なツール（`debugger`）が用意されています。

この 1 行をコードに追加すると、実行が一時停止され、DevTools の [**Sources**] パネルが開きます。ここから関数をステップ実行したり、オブジェクトを調査したり、コンソールを使用して現在のスコープに対してコマンドを実行したりできます。これは特に問題のある Service Worker のデバッグに役立ちます。

テストするには、`install` ハンドラをデバッグしてみましょう。

* `service-worker.js` の `install` ハンドラの先頭に `debugger` 文を追加します。

    self.addEventListener('install', function(event) {
      debugger;
      // Perform install steps
      event.waitUntil(
        caches.open(CACHE_NAME)
          .then(function(cache) {
            return cache.addAll(urlsToCache);
          })
      );  
    });
    

* [**Application**] パネルからページを更新します。

アプリは実行を一時停止して、パネルを `debugger` 文が `service-worker.js` でハイライト表示されている [**Sources**] に切り替えます。

![d960b322c020d6cc.png](img/2f20258491acfaa8.png)<aside class="key-point">

<p><strong>Learn More</strong>: A full explanation of the <strong>Sources</strong> panel is outside the scope of this codelab but you can  <a href="/web/tools/chrome-devtools/debug/">learn more about the debugging capabilities of the DevTools</a> on the Google Developers site.</p>

</aside> 

このビューで使用できる便利なツールは多数あります。そのようなツールの 1 つに [**Scope**] インスペクターがあります。これで現在の関数のスコープ内のオブジェクトの現在の状態を表示してみましょう。

* `event: ExtendableEvent` ドロップダウンをクリックします。

![5116146f838a566.png](img/3fa715abce820cea.png)

ここから現在のスコープ内のオブジェクトに関するあらゆる有用な情報を確認できます。たとえば、`type` フィールドを見ると、現在のイベント オブジェクトが `install` イベントであることを確認できます。

* [**Sources**] パネルで `service-worker.js` の 25 行目までスクロールし、行番号をクリックします。

![97cd70fb204fa26b.png](img/97cd70fb204fa26b.png)

ブレークポイントを設定するには、アプリが実行を停止する行番号をクリックする必要があります。

* ページを更新します。
* Click on **skipWaiting** to activate the new Service Worker

### ブレークポイントの代用

If you're already inspecting your code in the **Sources** panel, you may find it easier to set a breakpoint, versus adding `debugger` statements to your actual files. A breakpoint serves a similar purpose (it freezes execution and lets you inspect the app) but it can be set from within DevTools itself.

これにより、`fetch` ハンドラの先頭にブレークポイントが設定され、そのイベント オブジェクトを調査できます。

* [**Scope**] インスペクターで `event` オブジェクトを展開します。

![dabccb06c7231b3e.png](img/dabccb06c7231b3e.png)

This will set a breakpoint at the beginning of the `fetch` handler so you can inspect its event object.

* [**Resume**] ボタンを押してスクリプトの実行の続行を許可します。

この `FetchEvent` が `http://127.0.0.1:8887/`（`index.html`）のリソースをリクエストしていたことがわかります。アプリは多数の `fetch` リクエストを処理するため、ブレークポイントをそのままにして実行を再開します。これにより、アプリを通過する各 `FetchEvent` を調査できるようになります。アプリのすべてのリクエストを細かく確認するのに非常に役に立つ手法です。

* `service-worker.js` を開いて、`fetch` ハンドラの後に次の行を追加します。
* Expand the `request` object
* Note the `url` property

![ce7b5e8df4e8bc07.png](img/f9b0c00237b4400d.png)

しばらくすると、同じブレークポイントで実行が一時停止されます。`event.request.url` プロパティを確認して `http://127.0.0.1:8887/styles/main.css` が表示されていること確認します。このように `smiley.svg`、`main.js`、最後に `manifest.json` のリクエストの確認を続行できます。

* [**Application**] パネルを開きます。

![66b08c42b47a9987.png](img/66b08c42b47a9987.png)

アプリの中央に、ユーザーにプッシュ通知のサブスクライブを求めるボタンがあります。このボタンは、クリックされるとユーザーからプッシュ通知パーミッションをリクエストされるように既に連携されています。

When you are finished exploring, remove any breakpoints and comment out the `debugger` call so that they don't interfere with the rest of the lab.

## プッシュ通知のテスト

残りのステップは、`push` イベントのサポートを `service-worker.js` に追加するステップのみです。

### プッシュ サポートの追加

このハンドラを使用すると、Push イベントのシミュレートが簡単です。

![a8a8fa8d35b0667a.png](img/3e7f08f9d8c1fc5c.png)<aside class="warning">

<p>The code used to set up this Push subscription is just for demo purposes and should not be used in production. For a thorough guide on setting up Push notifications  <a href="/web/fundamentals/engage-and-retain/push-notifications/">see this post</a> on the Google Developers site.</p>

</aside> 

The only remaining step is to add support for the `push` event to `service-worker.js`.

* 最後に、[**Update**] と [**Unregister**] の横の [**Push**] ボタンをクリックします。

    self.addEventListener('push', function(event) {  
      var title = 'Yay a message.';  
      var body = 'We have received a push message.';  
      var icon = '/images/smiley.svg';  
      var tag = 'simple-push-example-tag';
      event.waitUntil(  
        self.registration.showNotification(title, {  
          body: body,  
          icon: icon,  
          tag: tag  
        })  
      );  
    });
    

画面の右上にプッシュ通知が表示されるので、Service Worker が想定どおりに `push` イベントを処理していることを確認します。

* Open the **Application** panel
* Refresh the page, when you see the new Service Worker enter the `waiting` phase, click on the **skipWaiting** button
* Click on the **Subscribe to Push Notifications** button in the app
* Accept the permission prompt

![b552ed129bc6cdf6.png](img/a8a8fa8d35b0667a.png)

* Finally, click the **Push** button, next to **Update** and **Unregister** back in the **Application** tab

![eacd4c5859f5f3ff.png](img/eacd4c5859f5f3ff.png)

ツールボックスにはいくつかのデバッグ ツールが用意され、プロジェクトに発生するどのような問題も解決できる知識を身に付けました。次は、魅力的な Progressive Web App を構築してみましょう。

![b552ed129bc6cdf6.png](img/b552ed129bc6cdf6.png)

{# wf_devsite_translation #}

Now that you have some debugging tools in your toolbox, you should be well equipped to fix-up any issues that arise in your project. The only thing left is for you to get out there and build the next amazing Progressive Web App!

## 問題の発見やフィードバック{: .hide-from-toc }

Help us make our code labs better by submitting an [issue](https://github.com/googlecodelabs/debugging-service-workers/issues) today. And thanks!