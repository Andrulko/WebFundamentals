project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: プログレッシブ ウェブアプリはウェブとアプリの両方の利点を兼ね備えたアプリです。このステップバイステップガイドでは、あなた自身のプログレッシブ ウェブアプリを構築し、そしてプログレッシブウェブアプリの開発で必要とされる基礎を学ぶことになります。それは、App Shellモデルや、App ShellやあなたのアプリケーションのキーデータなどをキャッシュするためのService Workerの使い方を含みます。

{# wf_auto_generated #} {# wf_updated_on: 2016-04-06 #} {# wf_published_on: 2016-02-04 #}

# はじめてのプログレッシブ ウェブアプリ {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

## プログレッシブ ウェブアプリとは

<a href="/web/progressive-web-apps">プログレッシブ ウェブアプリ</a>はウェブとアプリの両方の利点を兼ね備えたアプリです。ブラウザのタブで表示してすぐに利用することができ、インストールの必要はありません。使い続けてユーザーとの関係性が構築されていくにつれ、より強力なアプリとなります。不安定なネットワークでも迅速に起動し、関連性の高いプッシュ通知を送信することができます。また、ホーム画面にアイコンを表示することができ、トップレベルの全画面表示で読み込むことができます。

### App Shell アーキテクチャを使用する理由

プログレッシブ ウェブアプリには以下の特徴があります。

* **段階的** - プログレッシブ・エンハンスメントを基本理念としたアプリであるため、 ブラウザに関係なく、すべてのユーザーに利用してもらえます。
* **レスポンシブ** - パソコンでもモバイルでもタブレットでも、次世代の端末でも、 あらゆるフォームファクタに適合します。
* **ネットワーク接続に依存しない** - Service Worker の活用により、オフラインでも、 ネットワーク環境が良くない場所でも動作します。
* **アプリ感覚** - App Shell モデルに基づいて作られているため、アプリ感覚で操作できます。
* **常に最新** - Service Worker の更新プロセスにより、常に最新の状態に保たれます。
* **安全** - 覗き見やコンテンツの改ざんを防ぐため、HTTPS 経由で配信されます。
* **発見しやすい** - W3C のマニフェストとService Worker の登録スコープにより、 「アプリケーション」として認識されつつ、検索エンジンからも発見することができます。
* **再エンゲージメント可能** - プッシュ通知のような機能を通じで容易に 再エンゲージメントを促すことができます。
* **インストール可能** - ユーザーが気に入ればアプリのリンクをホーム画面に残して おくことができ、アプリストアで探し回る必要はありません。
* **リンク可能** - URL を使って簡単に共有でき、複雑なインストールの必要はありません。

このスタートガイドでは、独自のプログレッシブ ウェブアプリを作成する方法について、 設計時の考慮事項や、実装の詳細など、順を追って説明します。この説明に沿って作業することで、 プログレッシブ ウェブアプリの原則に基づいたアプリを作成することができます。<aside class="key-point">

<p>Looking for more? Check out the talks from the  <a href="https://www.youtube.com/playlist?list=PLNYkxOF6rcIAWWNR_Q6eLPhsyx6VvYjVb">2016 Progressive Web App Summit</a>.</p>

</aside> 

### App Shell を設計する

<table>
  <p>
    <tr>
      <td colspan="1" rowspan="1">
        </p>

<p>In this codelab, you're going to build a Weather web app using Progressive Web App techniques. Your app will:</p>

        
        <ul>
          
<li>Utilize and demonstrate the above principles of Progressive Web Apps.</li>
<li>Use live weather data.</li>
          
          <li>
            
<p>Provide app-like interactions to allow the user to add cities.</p>
</td><td colspan="1" rowspan="1">
              </li> </ul>

<p><img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png"></p>

              
              <p>
                </td> </tr>
              </p></table> 
              
              <h3>
                コードのダウンロード
              </h3>
              
              <ul>
                <li>
                  <b>段階的</b> - 徐々に機能が強化されていくようにします。
                </li>
                <li>
                  <b>レスポンシブ</b> - あらゆるフォームファクタに適合するようにします。
                </li>
                <li>
                  <b>ネットワーク接続に依存しない</b> - Service Worker で App Shell をキャッシュします。
                </li>
              </ul>
              
              <h3>
                App Shell の HTML を作成する
              </h3>
              
              <ul>
                <li>
                  「App Shell」方式に基づいてアプリを設計し構築する方法
                </li>
                <li>
                  アプリをオフラインで動作可能にする方法
                </li>
                <li>
                  <a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip">The sample code</a>
                </li>
                <li>
                  A text editor
                </li>
                <li>
                  Basic knowledge of HTML, CSS, JavaScript, and <a href="https://developer.chrome.com/devtools">Chrome DevTools</a>
                </li>
              </ul>
              
              <p>
                Note: さらに詳しくは、2015 年の Chrome Dev Summit で Alex Russell が行った、<a href='https://www.youtube.com/watch?v=MyQ8mtR9WxI'>プログレッシブ ウェブアプリ</a>についての講演内容をご覧ください。
              </p>
              
              <h2>
                作成するもの
              </h2>
              
              <h3>
                主要な UI コンポーネントにスタイルを追加する
              </h3>
              
              <p>
                このコードラボでは、プログレッシブ ウェブアプリの技法を使って お天気ウェブアプリを作成します。
              </p>
              
              <p>
                <a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip">Download source code</a>
              </p>
              
              <p>
                App Shell とは、プログレッシブ ウェブアプリのユーザー インターフェースが機能する ための最小限の HTML、CSS、JavaScript であり、高いパフォーマンスを発揮するために 必要な要素の 1 つです。最初の読み込みは高速で行われ、読み込み後すぐにキャッシュ されます。それ以降、毎回の読み込みは行われず、必要なコンテンツだけが取得されます。
              </p>
              
              <p>
                App Shell のアーキテクチャでは、アプリケーションの核となるインフラストラクチャと ユーザー インターフェースを、データから切り離して扱います。ユーザー インターフェース とインフラストラクチャはすべて、Service Worker によりローカルにキャッシュされる ので、以降の読み込み時にはすべてを読み込まなくても必要なデータだけを取得すればよい ことになります。
              </p>
              
              <h3>
                実行と調整
              </h3>
              
              <p>
                While you're free to use your own web server, this codelab is designed to work well with the Chrome Web Server. If you don't have that app installed yet, you can install it from the Chrome Web Store.
              </p>
              
              <p>
                <a href="https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb">Install Web Server for Chrome</a>
              </p>
              
              <p>
                App Shell アーキテクチャを採用すると、スピードを追及でき、プログレッシブ ウェブアプリにネイティブ アプリのような特性を持たせることができます。 つまり、アプリストアを一切介することなく、瞬時の読み込みや定期的な更新が可能です。
              </p>
              
              <p>
                <img src="img/9efdf0d1258b78e4.png" alt="9efdf0d1258b78e4.png" />
              </p><aside class="key-point">

<p>More help:  <a href="https://support.google.com/chrome_webstore/answer/3060053">Add and open Chrome apps</a></p>

</aside> 
              
              <p>
                次のことを考えてみてください。
              </p>
              
              <p>
                <img src="img/dc07bbc9fcfe7c5b.png" alt="dc07bbc9fcfe7c5b.png" />
              </p>
              
              <p>
                You'll see this dialog next, which allows you to configure your local web server:
              </p>
              
              <p>
                <img src="img/433870360ad308d4.png" alt="433870360ad308d4.png" />
              </p>
              
              <p>
                どのようなプロジェクトでも開始にはいくつかの方法がありますが、通常は Web Starter Kit の利用をおすすめしています。ただし今回は、プロジェクトをできるだけ 簡単なものにしてプログレッシブ ウェブアプリに集中できるように、必要なリソースを すべてご用意しました。
              </p>
              
              <p>
                簡単に使えるように、<a href="pwa-weather.zip">このプログレッシブ ウェブアプリガイドのすべてのコードをZIP ファイルとしてダウンロード</a>することができます。各ステップで必要となるすべての リソースがZIPファイル内から利用可能です。
              </p>
              
              <p>
                <img src="img/39b4e0371e9703e6.png" alt="39b4e0371e9703e6.png" />
              </p>
              
              <p>
                今回の構成要素をもう一度挙げます。
              </p>
              
              <p>
                <img src="img/daefd30e8a290df5.png" alt="daefd30e8a290df5.png" />
              </p>
              
              <p>
                次に、予報カードを追加し、そして New City ダイアログを追加しましょう。時間を節約 するために、それらは<code>resources</code>ディレクトリの中で提供されていますので、対応する場所 にそれらを簡単にコピーアンドペーストすることができます。
              </p>
              
              <p>
                <img src="img/aa64e93e8151b642.png" alt="aa64e93e8151b642.png" />
              </p>
              
              <p>
                <code>index.html</code>ファイル内で、<code>&lt;!-- Insert link to styles here --&gt;</code>を以下に 置き換えます。
              </p><aside class="key-point">

<p>From this point forward, all testing/verification (e.g. the<strong> Test It Out</strong> sections in subsequent steps) should be performed using this web server setup.</p>

</aside> 
              
              <h2>
                学習する内容
              </h2>
              
              <h3>
                主要な JavaScript ブートコードを追加する
              </h3>
              
              <p>
                時間を節約するために、あなたが使える<a href="https://weather-pwa-sample.firebaseapp.com/styles/inline.css">stylesheet</a> をすでに作成してあります。それをレビューし、そしてあなた自身でそれをカスタマイズする ために数分使ってください。
              </p>
              
              <p>
                Note: 個別に各アイコンを指定すると、画像のスプライトを使用する場合と比較して効率が悪く見えるかもしれませんが、アプリのシェルの一部として後でそれらをキャッシュし、ネットワーク要求をする必要なく常に利用可能であることを保証します。
              </p>
              
              <p>
                今が実行する絶好の時です。これらがどのような見た目になるかを見て、そしてあなたが 行いたい調整をしてください。<code>main</code>コンテナから<code>hidden</code>属性を削除し、そしてカードに 幾つかの架空のデータを追加することによって、予報カードの描画をテストしてください。
              </p>
              
              <p>
                <img src="img/156b5e3cc8373d55.png" alt="156b5e3cc8373d55.png" />
              </p>
              
              <p>
                このアプリは現状ほぼレスポンシブですが、完全ではありません。レスポンシブ性を改善し、 異なるデバイスを横断して本当に光り輝かせるために、追加のスタイルを加えてみてください。 また、あなた自身でできることを考えてみてください。
              </p>
              
              <h3>
                テスト
              </h3>
              
              <p>
                ここまでで、ユーザー インターフェースの大半が揃いました。次はすべてが動作するように コードを組み合わせます。App Shell の他の部分と同様に、中心的なエクスペリエンスを 実現するのに重要なコードがどれで、後で読み込むことのできるコードがどれかを意識して 作業してください。
              </p>
              
              <h3>
                天気予報データを挿入する
              </h3>
              
              <p>
                今回のブートコードには次の要素が含まれています。
              </p>
              
              <p>
                JavaScript コードを追加してください。
              </p>
              
              <ul>
                <li>
                  Chrome 47 以上
                </li>
                <li>
                  HTML、CSS 、JavaScript の基本知識
                </li>
                <li>
                  What supporting resources are needed for the app shell? For example images, JavaScript, styles, etc.
                </li>
              </ul>
              
              <p>
                基本の HTML、スタイル、JavaScript が揃ったので、アプリをテストしましょう。 この時点で行われる動作は限定的ですが、コンソールにエラーが書き込まれないことを確認 してください。
              </p>
              
              <table>
                <p>
                  <tr>
                    <td colspan="1" rowspan="1">
                      </p> 
                      
                      <ul>
                        
<li>Header with a title, and add/refresh buttons</li>
<li>Container for forecast cards</li>
<li>A forecast card template</li>
<li>A dialog box for adding new cities</li>
                        
                        <li>
                          
<p>A loading indicator</p>
</td><td colspan="1" rowspan="1">
                            </li> </ul>

<p><img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png"></p>

                            
                            <p>
                              </td> </tr>
                            </p></table> 
                            
                            <p>
                              架空の天気データがどのように表示されるかを確認するには、<code>app.js</code>ファイルの末尾に 次の行を追加してください。
                            </p>
                            
                            <h2>
                              必要なもの
                            </h2>
                            
                            <p>
                              There are multiple ways to get started with any project, in this case, to keep our project as simple as possible and concentrate on Progressive Web Apps, we've provided you with all of the resources you'll need.
                            </p>
                            
                            <h3>
                              初回実行時と区別する
                            </h3>
                            
                            <p>
                              Now we'll add the core components we discussed in <a href="/web/fundamentals/getting-started/codelabs/your-first-pwapp#architect_your_app_shell">Architect the App Shell</a>.
                            </p>
                            
                            <p>
                              プログレッシブ ウェブアプリは、高速に起動してすぐに使えるものでなければなりません。現在の状態では、お天気アプリは高速に起動しますが、データがないため使えるものになっていません。AJAX リクエストを使ってデータを取得することもできますが、それではリクエストを余分に行うことになり、最初の読み込みに時間がかかってしまいます。そこで、最初の読み込みでは実際のデータを指定します。
                            </p>
                            
                            <ul>
                              <li>
                                画面に即座に表示しなければならないものは？
                              </li>
                              <li>
                                その他、アプリに重要なユーザー インターフェース要素は？
                              </li>
                              <li>
                                App Shell に必要なサポート リソースは？（例: 画像、JavaScript、スタイル）
                              </li>
                              <li>
                                A dialog for adding new cities
                              </li>
                              <li>
                                A loading indicator
                              </li>
                            </ul>
                            
                            <p>
                              このコードラボでは天気予報の静的データをあらかじめ指定します。ただし本番のアプリでは、 ユーザーの IP アドレスから判定できる地域情報に基づいて、最新の天気予報データを サーバーから挿入することになります。
                            </p>
                            
                            <pre><code>&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;meta charset="utf-8"&gt;
  &lt;meta http-equiv="X-UA-Compatible" content="IE=edge"&gt;
  &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
  &lt;title&gt;Weather App&lt;/title&gt;
  &lt;!-- Insert link to styles.css here --&gt;
&lt;/head&gt;
&lt;body&gt;
  &lt;header class="header"&gt;
    &lt;h1 class="header__title"&gt;Weather App&lt;/h1&gt;
    &lt;button id="butRefresh" class="headerButton"&gt;&lt;/button&gt;
    &lt;button id="butAdd" class="headerButton"&gt;&lt;/button&gt;
  &lt;/header&gt;

  &lt;main class="main" hidden&gt;
    &lt;!-- Insert forecast-card.html here --&gt;
  &lt;/main&gt;

  &lt;div class="dialog-container"&gt;
    &lt;!-- Insert add-new-city-dialog.html here --&gt;
  &lt;/div&gt;

  &lt;div class="loader"&gt;
    &lt;svg viewBox="0 0 32 32" width="32" height="32"&gt;
      &lt;circle id="spinner" cx="16" cy="16" r="14" fill="none"&gt;&lt;/circle&gt;
    &lt;/svg&gt;
  &lt;/div&gt;

  &lt;!-- Insert link to app.js here --&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>
                            
                            <p>
                              即時呼び出しの関数式の内部に以下のコードを追加します。
                            </p>
                            
                            <p>
                              次に、前にテストのために作成した<code>fakeForecast</code>データはもう使うことはないので、削除します。
                            </p><aside class="key-point">

<p>We've given you the markup and styles to save you some time and make sure you're starting on a solid foundation. In the next section, you'll have an opportunity to write your own code.</p>

</aside> 
                            
                            <h3>
                              テスト
                            </h3>
                            
                            <p>
                              さて、前述の情報を表示するタイミングはどのように判断するのでしょうか。今後お天気 アプリがキャッシュから取得されて読み込まれるとき、この情報の関連性は失われている かもしれません。ユーザーが次にアプリを読み込むときには都市が変わっている可能性も あります。そのため、これまでに確認された都市に限らず、該当する都市の情報を読み込む 必要があります。
                            </p>
                            
                            <p>
                              ユーザーが登録した都市のリストのようなユーザー設定は、IndexedDB などの高速な ストレージ システムを利用してローカルに保存しておく必要があります。今回はできるだけ 簡単な例にするために <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage">localStorage</a> を使用しましたが、これは本番のアプリには適していません。<code>localStorage</code> では ブロッキングな同期の仕組みが使われており、端末によっては著しくスピードが低下する 可能性があるためです。
                            </p>
                            
                            <ul>
                              <li>
                                タイトル ヘッダー、追加 / 更新ボタン
                              </li>
                              <li>
                                予報カードのコンテナ
                              </li>
                              <li>
                                予報カードのテンプレート
                              </li>
                              <li>
                                都市の追加用ダイアログ ボックス
                              </li>
                              <li>
                                読み込みインジケータ
                              </li>
                              <li>
                                Some fake data (<code>initialWeatherForecast</code>) you can use to quickly test how things render.
                              </li>
                            </ul>
                            
                            <h3>
                              Service Worker が利用可能な場合に登録する
                            </h3>
                            
                            <p>
                              Note: 補習: <code>localstorage</code> の実装を <a href='https://www.npmjs.com/package/idb'>idb</a> に置き替えてみましょう。
                            </p>
                            
                            <p>
                              まず、<code>app.js</code> 内の即時呼び出しの関数式の最後に、ユーザー設定の保存に必要な コードを追加します。
                            </p>
                            
                            <pre><code>&lt;link rel="stylesheet" type="text/css" href="styles/inline.css"&gt;
</code></pre>
                            
                            <p>
                              次に、スタートアップ コードを追加します。このコードでは、ユーザーが登録している 都市があるか確認してその都市を読み込むか、サーバーからのデータを使用します。 <code>app.js</code> ファイル内の、先程追加したコードの後に次のコードを追加しましょう。
                            </p>
                            
                            <pre><code>var initialWeatherForecast = {
  key: 'newyork',
  label: 'New York, NY',
  currently: {
    time: 1453489481,
    summary: 'Clear',
    icon: 'partly-cloudy-day',
    temperature: 52.74,
    apparentTemperature: 74.34,
    precipProbability: 0.20,
    humidity: 0.77,
    windBearing: 125,
    windSpeed: 1.52
  },
  daily: {
    data: [
      {icon: 'clear-day', temperatureMax: 55, temperatureMin: 34},
      {icon: 'rain', temperatureMax: 55, temperatureMin: 34},
      {icon: 'snow', temperatureMax: 55, temperatureMin: 34},
      {icon: 'sleet', temperatureMax: 55, temperatureMin: 34},
      {icon: 'fog', temperatureMax: 55, temperatureMin: 34},
      {icon: 'wind', temperatureMax: 55, temperatureMin: 34},
      {icon: 'partly-cloudy-day', temperatureMax: 55, temperatureMin: 34}
    ]
  }
};
</code></pre>
                            
                            <p>
                              最後に、ユーザーが新しい都市を追加したときには必ず都市のリストを保存することを 忘れないでください。それには、<code>butAddCity</code> ボタンのイベント ハンドラに <code>app.saveSelectedCities();</code> を追加します。
                            </p>
                            
                            <p>
                              <img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-04/">TRY IT</a>
                            </p>
                            
                            <p>
                              Service Worker をよくご存知ない場合は、<a href="/web/fundamentals/getting-started/primers/service-workers">Service Worker の概要記事</a>をご覧ください。 この記事では、Service Worker でできることや、Service Worker のライフサイクルなど、 基本事項を説明しています。
                            </p>
                            
                            <h2>
                              App Shellを構築する
                            </h2>
                            
                            <p>
                              Service Worker を介して提供する機能は、プログレッシブ・エンハンスメントの 1 つとして 考えるべきで、こうした機能を追加するのはブラウザでサポートされている場合のみにする 必要があります。たとえばネットワークを使用できない状況では、Service Worker を使って App Shell とアプリのデータをキャッシュすることができます。一方 Service Worker がサポートされていない場合は、オフラインのコードを呼び出すことなく、最小限のユーザー エクスペリエンスのみを提供します。段階的な機能向上を提供するための機能検出に伴う オーバーヘッドはわずかで、機能をサポートしていない古いブラウザが使用されている場合、 問題が起こることはありません。
                            </p>
                            
                            <h3>
                              サイトのアセットをキャッシュする
                            </h3>
                            
                            <p>
                              Note: Service Worker の機能は HTTPS 経由でアクセスしたページでのみ使用できます（テストを円滑に進められるように、<code>https://localhost</code> またはこれに相当する URL でも動作するようになっています）。この制約が課せられる理由については、Chromium チームの投稿記事 <a href='http://www.chromium.org/Home/chromium-security/prefer-secure-origins-for-powerful-new-features'>Prefer Secure Origins For Powerful New Features</a>（強力な新機能に対する「セキュア オリジン」の採用傾向について）をご覧ください。
                            </p>
                            
                            <p>
                              オフラインでもアプリが動作するようにするには、まず Service Worker を登録します。 Service Worker は、ウェブページを開いていなくても、またはユーザーの操作がなくても、 バックグラウンドで処理を進めることのできるスクリプトです。
                            </p>
                            
                            <h3>
                              キャッシュからApp Shell を配信する
                            </h3>
                            
                            <p>
                              登録の手順は次のとおりです。
                            </p>
                            
                            <p>
                              まず、アプリケーションのルートフォルダ（<code>your-first-pwapp-master/work</code>）に <code>service-worker.js</code> という空のファイルを作成します。このファイルは アプリケーションのルートに置く必要があります。このファイルが置かれている ディレクトリによって Service Worker のスコープが定義されるためです。
                            </p><aside class="key-point">

<p><strong>Extra Credit</strong>: Replace <code>localStorage</code> implementation with  <a href="https://www.npmjs.com/package/idb">idb</a>, check out  <a href="https://github.com/localForage/localForage">localForage</a> as a simple wrapper to idb.</p>

</aside> 
                            
                            <p>
                              次に、ブラウザで Service Worker がサポートされているかどうかを確認し、サポート されている場合は Service Worker を登録します。方法は、 <code>app.js</code> ファイルに、 次のコードを追加します。
                            </p>
                            
                            <pre><code>  // Save list of cities to localStorage, see note below about localStorage.
app.saveSelectedCities = function() {
  var selectedCities = JSON.stringify(app.selectedCities);
  // IMPORTANT: See notes about use of localStorage.
  localStorage.selectedCities = selectedCities;
};
</code></pre>
                            
                            <p>
                              Service Worker を登録すると、ユーザーがページに初めてアクセスしたときに <code>インストール</code> イベントが呼び出されます。このイベント ハンドラで、アプリケーションに 必要なすべてのアセットをキャッシュします。
                            </p>
                            
                            <pre><code>  /****************************************************************************
 *

 * Code required to start the app
 *
 * NOTE: To simplify this getting started guide, we've used localStorage.
 *   localStorage is a synchronous API and has serious performance
 *   implications. It should not be used in production applications!
 *   Instead, check out IDB (https://www.npmjs.com/package/idb) or
 *   SimpleDB (https://gist.github.com/inexorabletash/c8069c042b734519680c)
 *
 ****************************************************************************/

app.selectedCities = localStorage.selectedCities;
if (app.selectedCities) {
  app.selectedCities = JSON.parse(app.selectedCities);
  app.selectedCities.forEach(function(city) {
    app.getForecast(city.key, city.label);
  });
} else {
  app.updateForecastCard(initialWeatherForecast);
  app.selectedCities = [
    {key: initialWeatherForecast.key, label: initialWeatherForecast.label}
  ];
  app.saveSelectedCities();
}
</code></pre>
                            
                            <p>
                              Note: 下記のコードは本番環境では使用<b>しないでください</b>。これはごく基本的な用途に対応したコードで、本番環境で使用すると App Shell が更新されない状況に陥る可能性があります。後のセクションでこの実装に伴う危険性とその回避方法を説明していますので、必ずご確認ください。
                            </p>
                            
                            <pre><code>  if('serviceWorker' in navigator) {
  navigator.serviceWorker
           .register('/service-worker.js')
           .then(function() { console.log('Service Worker Registered'); });
}
</code></pre>
                            
                            <p>
                              Service Worker が呼び出されると、caches オブジェクトが開かれ、App Shell の 読み込みに必要なアセットが挿入されます。<code>service-worker.js</code> ファイルの末尾に次の コードを追加してください。
                            </p>
                            
                            <pre><code>var cacheName = 'weatherPWA-step-5-1';
var filesToCache = [];

self.addEventListener('install', function(e) {
  console.log('[ServiceWorker] Install');
  e.waitUntil(
    caches.open(cacheName).then(function(cache) {
      console.log('[ServiceWorker] Caching app shell');
      return cache.addAll(filesToCache);
    })
  );
});
</code></pre>
                            
                            <p>
                              まず、<code>caches.open()</code> を使用してキャッシュを開き、キャッシュに名前を付けます。 キャッシュに名前を付けることでファイルのバージョン管理が可能になります。また、 データと App Shell を切り離し、お互いに影響を与えることなく個別に更新できるように なります。
                            </p>
                            
                            <h3>
                              特殊なケースに関する注意
                            </h3>
                            
                            <p>
                              キャッシュが開いたら、<code>cache.addAll()</code> を呼び出します。これは URL のリストを取り、 該当のファイルをサーバーから取得して応答をキャッシュに追加します。<code>cache.addAll()</code> は最小単位であるため、ファイルのうち 1 つでも取得できないものがあると、キャッシュの ステップそのものが失敗に終わります。
                            </p>
                            
                            <p>
                              Service Worker に変更を加えるときには必ず <code>cacheName</code> を変更し、キャッシュから 最新版のファイルが取得されるようにします。使わないコンテンツやデータのキャッシュは 定期的に削除することが重要です。イベント リスナーを追加して、すべてのキャッシュキー の取得と使われていないキャッシュキーの削除を行う <code>activate</code> イベントを待機します。
                            </p>
                            
                            <pre><code>self.addEventListener('activate', function(e) {
  console.log('[ServiceWorker] Activate');
  e.waitUntil(
    caches.keys().then(function(keyList) {
      return Promise.all(keyList.map(function(key) {
        console.log('[ServiceWorker] Removing old cache', key);
        if (key !== cacheName) {
          return caches.delete(key);
        }
      }));
    })
  );
});
</code></pre>
                            
                            <p>
                              最後に、App Shell に必要なファイルのリストを更新しましょう。画像、JavaScript、 スタイルシートなど、アプリに必要なすべてのファイルを配列に含めます。
                            </p>
                            
                            <h3>
                              運用中の Service Worker をテストする際のヒント
                            </h3>
                            
                            <ul>
                              <li>
                                タイトル ヘッダー、追加 / 更新ボタン
                              </li>
                              <li>
                                予報カードのコンテナ
                              </li>
                              <li>
                                予報カードのテンプレート
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-05/">TRY IT</a>
                            </p>
                            
                            <h2>
                              App Shell を実装する
                            </h2>
                            
                            <p>
                              まだアプリはオフラインで動作しません。App Shell の構成要素のキャッシュは できましたが、ローカル キャッシュからの読み込みを行う必要があります。
                            </p>
                            
                            <p>
                              Service Worker を使うと、プログレッシブ ウェブアプリから送信されたリクエストを 傍受して Service Worker 内部で処理することができます。つまり、リクエストの 処理方法を決めることができ、キャッシュした応答を配信することも可能です。
                            </p>
                            
                            <p>
                              例:
                            </p><aside class="key-point">

<p><strong>Remember</strong>: Service worker functionality is only available on pages that are accessed via HTTPS (<a href="http://localhost">http://localhost</a> and equivalents will also work, to facilitate testing). To learn about the rationale behind this restriction check out  <a href="http://www.chromium.org/Home/chromium-security/prefer-secure-origins-for-powerful-new-features">Prefer Secure Origins For Powerful New Features</a> from the Chromium team.</p>

</aside> 
                            
                            <h3>
                              テスト
                            </h3>
                            
                            <p>
                              <code>service-worker.js</code> file: それでは、キャッシュからApp Shell を配信しましょう。<code>service-worker.js</code> ファイルの末尾に次のコードを追加します。
                            </p>
                            
                            <p>
                              内側から外側に向かって説明すると、まず <code>caches.match()</code> を使用して、<code>fetch</code> イベントを呼び出したウェブ リクエストを評価し、キャッシュからのデータが利用可能か どうかを確認します。次に、キャッシュ データで応答するか、fetch を使用して ネットワークからコピーを取得します。そして、<code>e.respondWith()</code> を使用して ウェブページに <code>response</code> を返します。
                            </p>
                            
                            <ol start="1">
                              <li>
                                <code>resources</code>ディレクトリからあなたの<code>scripts</code>フォルダへ<code>step3-app.js</code>ファイルを コピーして、それを<code>app.js</code>に名前変更してください。
                              </li>
                              
                              <li>
                                <code>index.html</code>ファイル内で、新しく作られた<code>app.js</code>へのリンクを追加してください。<br /> <code>&lt;script src="/scripts/app.js"&gt;&lt;/script&gt;</code>
                              </li>
                            </ol>
                            
                            <p>
                              Note: <code>[ServiceWorker]</code> がコンソールにログ出力されない場合は、<code>cacheName</code> を変更していることを確認してページを再度読み込んでください。これで解決できない場合は、「運用中の Service Worker のテストを行う際のヒント」の項をご覧ください。
                            </p>
                            
                            <pre><code>  var filesToCache = [
  '/',
  '/index.html',
  '/scripts/app.js',
  '/styles/inline.css',
  '/images/clear.png',
  '/images/cloudy-scattered-showers.png',
  '/images/cloudy.png',
  '/images/fog.png',
  '/images/ic\_add\_white\_24px.svg',
  '/images/ic\_refresh\_white\_24px.svg',
  '/images/partly-cloudy.png',
  '/images/rain.png',
  '/images/scattered-showers.png',
  '/images/sleet.png',
  '/images/snow.png',
  '/images/thunderstorm.png',
  '/images/wind.png'
];
</code></pre>
                            
                            <h3>
                              ネットワーク リクエストを傍受して応答をキャッシュする
                            </h3>
                            
                            <p>
                              何度も言いますが、このコードは<strong>本番環境では使用しないでください</strong>。このコードは、 多くの特殊ケースには対応していません。
                            </p><aside class="warning">

<p>The code below must NOT be used in production, it covers only the most basic use cases and it's easy to get yourself into a state where your app shell will never update. Be sure to review the section below that discusses the pitfalls of this implementation and how to avoid them.</p>

</aside> 
                            
                            <p>
                              たとえば、このキャッシュ方法では、コンテンツを変更するたびにキャッシュキーを更新する 必要があります。そうしないとキャッシュは更新されず、古いコンテンツが配信されることに なります。このため、プロジェクトでの作業中は、変更を行うたびにキャッシュキーを変更 するようにしてください。
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(event) {
  // Do something interesting with the fetch here
});
</code></pre>
                            
                            <p>
                              もう 1 つの注意点は、ファイルを変更するとキャッシュ全体が無効になるため、 再ダウンロードが必要になるということです。つまり、1 文字のスペルミスを修正した だけでも、キャッシュが無効になり、もう一度全体をダウンロードしなければならなく なります。これはあまり効率的とは言えません。
                            </p>
                            
                            <p>
                              さらにもう 1 つの注意点として、インストール処理中に行う HTTPS リクエストは ネットワークに直接送信し、ブラウザのキャッシュから応答が返されないようにすることが 重要です。そうしないと、キャッシュされた古い応答がブラウザから返され、その結果、 Service Worker のキャッシュが更新されなくなります。
                            </p>
                            
                            <p>
                              今回のアプリでは「キャッシュ優先」の戦略を使用します。この場合、キャッシュされた コンテンツのコピーがあれば、ネットワークに問い合わせを行わずにキャッシュのコピーを 返すことになります。「キャッシュ優先」の戦略は簡単に実装できる一方で、後から さまざまな課題を生む原因になることがあります。ホストページと Service Worker の登録内容のコピーがキャッシュされると、Service Worker の設定を変更することは 極めて困難です（設定は定義された場所に依存するため）。また、実装したサイトの更新も 非常に複雑になります。
                            </p>
                            
                            <p>
                              <img src="img/ed4633f91ec1389f.png" alt="ed4633f91ec1389f.png" />
                            </p>
                            
                            <p>
                              Service Worker のデバッグは困難な場合があります。さらに、キャッシュを使用する 場合に想定どおりにキャッシュが更新されないと、さらに解決に苦労することになります。 典型的な Service Worker のライフサイクルとコードのバグに挟まれて、身動きが とれなくなってしまうでしょう。しかし、こうした作業を容易にしてくれるツールが あります。
                            </p>
                            
                            <p>
                              ヒント:
                            </p>
                            
                            <p>
                              <img src="img/bf15c2f18d7f945c.png" alt="bf15c2f18d7f945c.png" />
                            </p>
                            
                            <p>
                              When you see information like this, it means the page has a service worker running.
                            </p>
                            
                            <p>
                              データに正しいキャッシュ戦略を選択することは重要であり、これはアプリで提供する データの種類によって決まります。たとえば、天気情報や株価など時間の経過とともに 変動するデータはできるだけ最新のものでなければなりませんが、アバターの画像や記事の コンテンツなどは更新の頻度が比較的少なくても問題はないと考えられます。
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(e) {
  console.log('[ServiceWorker] Fetch', e.request.url);
  e.respondWith(
    caches.match(e.request).then(function(response) {
      return response || fetch(e.request);
    })
  );
});
</code></pre>
                            
                            <p>
                              今回のアプリに適しているのは、<strong>まずキャッシュ、次にネットワークという優先</strong>順でデータを 取得する戦略です。この戦略では、画面にとにかく早くデータを表示し、その後ネットワーク から最新のデータが返された時点でデータの更新を行います。<strong>キャッシュではなく ネットワークを優先</strong>した場合、ネットワークからの fetch がタイムアウトになってから キャッシュ データが取得されることになり、待ち時間が発生してしまいます。キャッシュ 優先の場合はこうした待ち時間がなくなります。
                            </p>
                            
                            <p>
                              キャッシュ、ネットワークの順でデータを取得するには、キャッシュに 1 回、 ネットワークに 1 回、合計 2 回の非同期リクエストを送信する必要があります。 アプリのネットワーク リクエストにはそれほど変更を加える必要はありませんが、 Service Worker には、応答を返す前にキャッシュを行うよう変更を加える必要が あります。
                            </p>
                            
                            <p>
                              <img src="img/1f454b6807700695.png" alt="1f454b6807700695.png" />
                            </p>
                            
                            <p>
                              Service Worker に対し、Weather API へのリクエストを傍受するように、また後の アクセスを容易にするためその応答を Cache に格納するように変更を加えます。 <strong>キャッシュ、ネットワークの順</strong>にデータを取得する戦略では、ネットワークの応答を 「確実な情報源」として想定し、常に最新の情報を提供するものとして位置づけます。 ネットワークからデータを取得できない場合は、アプリで最新のキャッシュ データを取得 しているので、ネットワークで失敗しても問題はないということになります。
                            </p>
                            
                            <p>
                              Service Worker に <code>dataCacheName</code> を追加し、アプリケーションのデータと App Shell を切り離せるように設定しましょう。こうすると、App Shell が更新されて 古いキャッシュが消去されても、データは変更されず高速な読み込みに対応できます。なお、 将来データ形式が変わった場合は、App Shell とコンテンツの同期を確保しつつ新しい 形式に対応する方法が必要になります。
                            </p>
                            
                            <p>
                              <code>service-worker.js</code> ファイルの先頭に次の行を追加します。
                            </p>
                            
                            <p>
                              <img src="img/b1728ef310c444f5.png" alt="b1728ef310c444f5.png" />
                            </p>
                            
                            <p>
                              このコードでは、リクエストを傍受し、URL の先頭が Weather API のアドレスかどうかを 確認します。URL の先頭が Weather API のアドレスであれば、<code>fetch</code> を使用して リクエストを行います。応答が返されたらキャッシュを開き、応答をコピーして格納した後、 リクエストの送信元に応答を返します。
                            </p>
                            
                            <p>
                              次に、コードの <code>// Put data handler code here</code> の部分を以下のコードに 置き換えます。
                            </p>
                            
                            <pre><code>var dataCacheName = 'weatherData-v1';
</code></pre>
                            
                            <p>
                              このアプリはまだオフラインでは動作しません。App Shell のデータのキャッシュと取得を 実装しましたが、データをキャッシュできてもまだネットワークに依存している状態です。
                            </p>
                            
                            <p>
                              前に説明したとおり、アプリではキャッシュに 1 回、ネットワークに 1 回、合計 2 回の 非同期リクエストを送信する必要があります。アプリでは <code>window</code> で利用可能な <code>caches</code> オブジェクトを使ってキャッシュにアクセスし、最新のデータを取得します。 これはプログレッシブ・エンハンスメントを実装する場合の良い例です。すべてのブラウザで <code>caches</code> オブジェクトが利用可能とは限らず、<code>caches</code> オブジェクトが利用できない ときはネットワーク リクエストが引き続き動作可能でなければならないからです。
                            </p><aside class="key-point">

<p>When the app is complete, <code>self.clients.claim()</code> fixes a corner case in which the app wasn't returning the latest data. You can reproduce the corner case by commenting out the line below and then doing the following steps: 1) load app for first time so that the initial New York City data is shown 2) press the refresh button on the app 3) go offline 4) reload the app. You expect to see the newer NYC data, but you actually see the initial data. This happens because the service worker is not yet activated. <code>self.clients.claim()</code> essentially lets you activate the service worker faster.</p>

</aside> 
                            
                            <p>
                              必要な手順は次のとおりです。
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(e) {
  console.log('[ServiceWorker] Fetch', e.request.url);
  var dataUrl = 'https://publicdata-weather.firebaseio.com/';
  if (e.request.url.indexOf(dataUrl) === 0) {
    // Put data handler code here
  } else {
    e.respondWith(
      caches.match(e.request).then(function(response) {
        return response || fetch(e.request);
      })
    );
  }
});
</code></pre><aside class="key-point">

<p>Be sure to include all permutations of file names, for example our app is served from <code>index.html</code>, but it may also be requested as <code>/</code> since the server sends <code>index.html</code> when a root folder is requested. You could deal with this in the <code>fetch</code> method, but it would require special casing which may become complex.</p>

</aside> 
                            
                            <p>
                              まれに、キャッシュよりも先に XHR が応答することがあります。このような場合に キャッシュによってアプリが更新されないように、まずフラグを追加しましょう。<code>app</code> オブジェクトに <code>hasRequestPending: false</code> を追加します。
                            </p>
                            
                            <h3>
                              リクエストを行う
                            </h3>
                            
                            <p>
                              次に、<code>caches</code> オブジェクトが存在するかどうかを確認し、存在する場合はそこから 最新のデータをリクエストします。方法は、XHRが作られる前に、<code>app.getForecast</code> に 次のコードを追加します。
                            </p>
                            
                            <p>
                              最後に、<code>app.hasRequestPending</code> フラグを更新します。それには、XHR の作成の前に <code>app.hasRequestPending = true;</code> を追加し、XHR の応答ハンドラで <code>app.updateForecastCard(response)</code> の直前に <code>app.hasRequestPending = false;</code> と設定します。
                            </p>
                            
                            <pre><code>e.respondWith(
  fetch(e.request)
    .then(function(response) {
      return caches.open(dataCacheName).then(function(cache) {
        cache.put(e.request.url, response.clone());
        console.log('[ServiceWorker] Fetched&Cached Data');
        return response;
      });
    })
);
</code></pre>
                            
                            <p>
                              これで、お天気アプリでは、キャッシュから 1 回、XHR を介して 1 回、合計 2 回の 非同期リクエストが行われるようになりました。キャッシュにデータが存在する場合は そのデータが返され、XHR からの応答がなければキャッシュ データが高速（10s/ms） に表示されてカードが更新されます。その後、XHR から応答があると、Weather API から直接取得した最新のデータを使ってカードが更新されます。
                            </p>
                            
                            <pre><code>if ('caches' in window) {
  caches.match(url).then(function(response) {
    if (response) {
      response.json().then(function(json) {
        // Only update if the XHR is still pending, otherwise the XHR
        // has already returned and provided the latest data.
        if (app.hasRequestPending) {
          console.log('updated from cache');
          json.key = key;
          json.label = label;
          app.updateForecastCard(json);
        }
      });
    }
  });
}
</code></pre>
                            
                            <p>
                              何らかの理由でキャッシュより早く XHR から応答があった場合は、<code>hasRequestPending</code> フラグにより、ネットワークの最新データにキャッシュ データが上書きされる事態が回避されます。
                            </p><aside class="warning">

<p>If you're not seeing the <code>[ServiceWorker]</code> logging in the console, be sure you've changed the <code>cacheName</code> variable and that you're inspecting the right service worker by opening the Service Worker pane in the Applications panel and clicking <strong>inspect</strong> on the running service worker. If that doesn't work, see the section on Tips for testing live service workers.</p>

</aside> 
                            
                            <h3>
                              テスト
                            </h3>
                            
                            <p>
                              Your app is now offline-capable! Let's try it out.
                            </p>
                            
                            <p>
                              Translated By: {% include "web/_shared/contributors/yoichiro.html" %}
                            </p>
                            
                            <p>
                              <img src="img/ab9c361527825fac.png" alt="ab9c361527825fac.png" />
                            </p>
                            
                            <p>
                              Now, let's test out offline mode. Go back to the <strong>Service Worker</strong> pane of DevTools and enable the <strong>Offline</strong> checkbox. After enabling it, you should see a little yellow warning icon next to the <strong>Network</strong> panel tab. This indicates that you're offline.
                            </p>
                            
                            <p>
                              <img src="img/7656372ff6c6a0f7.png" alt="7656372ff6c6a0f7.png" />
                            </p>
                            
                            <p>
                              Reload your page and... it works! Kind of, at least. Notice how it loads the initial (fake) weather data.
                            </p>
                            
                            <p>
                              <img src="img/8a959b48e233bc93.png" alt="8a959b48e233bc93.png" />
                            </p>
                            
                            <p>
                              Check out the <code>else</code> clause in <code>app.getForecast()</code> to understand why the app is able to load the fake data.
                            </p>
                            
                            <p>
                              The next step is to modify the app and service worker logic to be able to cache weather data, and return the most recent data from the cache when the app is offline.
                            </p>
                            
                            <p>
                              <strong>Tip:</strong> To start fresh and clear all saved data (<code>localStorage</code>, <code>indexedDB</code> data, cached files) and remove any service workers, use the Clear storage pane in the Application tab.
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-06/">TRY IT</a>
                            </p>
                            
                            <h3>
                              Beware of the edge cases
                            </h3>
                            
                            <p>
                              As previously mentioned, this code <strong>must not be used in production</strong> because of the many unhandled edge cases.
                            </p>
                            
                            <h4>
                              変更のたびにキャッシュキーの更新が必要
                            </h4>
                            
                            <p>
                              For example this caching method requires you to update the cache key every time content is changed, otherwise, the cache will not be updated, and the old content will be served. So be sure to change the cache key with every change as you're working on your project!
                            </p>
                            
                            <h4>
                              変更のたびにキャッシュ全体の再ダウンロードが必要
                            </h4>
                            
                            <p>
                              Another downside is that the entire cache is invalidated and needs to be re-downloaded every time a file changes. That means fixing a simple single character spelling mistake will invalidate the cache and require everything to be downloaded again. Not exactly efficient.
                            </p>
                            
                            <h4>
                              ブラウザ キャッシュによってService Worker のキャッシュ更新が妨害される
                            </h4>
                            
                            <p>
                              There's another important caveat here. It's crucial that the HTTPS request made during the install handler goes directly to the network and doesn't return a response from the browser's cache. Otherwise the browser may return the old, cached version, resulting in the service worker cache never actually updating!
                            </p>
                            
                            <h4>
                              本番環境での「キャッシュ優先」戦略の使用
                            </h4>
                            
                            <p>
                              Our app uses a cache-first strategy, which results in a copy of any cached content being returned without consulting the network. While a cache-first strategy is easy to implement, it can cause challenges in the future. Once the copy of the host page and service worker registration is cached, it can be extremely difficult to change the configuration of the service worker (since the configuration depends on where it was defined), and you could find yourself deploying sites that are extremely difficult to update!
                            </p>
                            
                            <h4>
                              特殊なケースを回避するには
                            </h4>
                            
                            <p>
                              So how do we avoid these edge cases? Use a library like <a href="https://workboxjs.org/">Workbox</a>, which provides fine control over what gets expired, ensures requests go directly to the network and handles all of the hard work for you.
                            </p>
                            
                            <h3>
                              Tips for testing live service workers
                            </h3>
                            
                            <p>
                              Debugging service workers can be a challenge, and when it involves caching, things can become even more of a nightmare if the cache isn't updated when you expect it. Between the typical service worker life cycle and bug in your code, you may become quickly frustrated. But don't. There are some tools you can use to make your life easier.
                            </p>
                            
                            <h4>
                              作業に役立つページ: chrome://serviceworker-internals
                            </h4>
                            
                            <p>
                              In some cases, you may find yourself loading cached data or that things aren't updated as you expect. To clear all saved data (localStorage, indexedDB data, cached files) and remove any service workers, use the Clear storage pane in the Application tab.
                            </p>
                            
                            <p>
                              Some other tips:
                            </p>
                            
                            <ul>
                              <li>
                                アプリに必要な基本情報を含んでいる<code>app</code>オブジェクト。
                              </li>
                              <li>
                                すべてのボタンのイベント リスナー。ヘッダーのボタン（<code>add</code>/<code>refresh</code>）と、 都市の追加ダイアログのボタン（<code>add</code>/<code>cancel</code>）があります。
                              </li>
                              <li>
                                予報カードを追加または更新するためののメソッド（<code>app.updateForecastCard</code>）。
                              </li>
                              <li>
                                Firebase Public Weather API から最新の天気予報データを取得するためのメソッド （<code>app.getForecast</code>）。
                              </li>
                            </ul>
                            
                            <h2>
                              最初の読み込みを高速に行えるようにする
                            </h2>
                            
                            <p>
                              Choosing the right <a href="https://jakearchibald.com/2014/offline-cookbook/">caching strategy</a> for your data is vital and depends on the type of data your app presents. For example, time-sensitive data like weather or stock quotes should be as fresh as possible, while avatar images or article content can be updated less frequently.
                            </p>
                            
                            <p>
                              The <a href="https://jakearchibald.com/2014/offline-cookbook/#cache-network-race">cache-first-then-network</a> strategy is ideal for our app. It gets data on screen as quickly as possible, then updates that once the network has returned the latest data. In comparison to network-first-then-cache, the user does not have to wait until the <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">fetch</a> times out to get the cached data.
                            </p>
                            
                            <p>
                              Cache-first-then-network means we need to kick off two asynchronous requests, one to the cache and one to the network. Our network request with the app doesn't need to change much, but we need to modify the service worker to cache the response before returning it.
                            </p>
                            
                            <p>
                              Under normal circumstances, the cached data will be returned almost immediately providing the app with recent data it can use. Then, when the network request returns, the app will be updated using the latest data from the network.
                            </p>
                            
                            <h3>
                              Intercept the network request and cache the response
                            </h3>
                            
                            <p>
                              We need to modify the service worker to intercept requests to the weather API and store their responses in the cache, so we can easily access them later. In the cache-then-network strategy, we expect the network response to be the ‘source of truth', always providing us with the most recent information. If it can't, it's OK to fail because we've already retrieved the latest cached data in our app.
                            </p>
                            
                            <p>
                              In the service worker, let's add a <code>dataCacheName</code> so that we can separate our applications data from the app shell. When the app shell is updated and older caches are purged, our data will remain untouched, ready for a super fast load. Keep in mind, if your data format changes in the future, you'll need a way to handle that and ensure the app shell and content stay in sync.
                            </p>
                            
                            <p>
                              Add the following line to the top of your <code>service-worker.js</code> file:
                            </p>
                            
                            <pre><code>var dataCacheName = 'weatherData-v1';
</code></pre>
                            
                            <p>
                              Next, update the <code>activate</code> event handler so that it doesn't delete the data cache when it cleans up the app shell cache.
                            </p>
                            
                            <pre><code>if (key !== cacheName && key !== dataCacheName) {
</code></pre>
                            
                            <p>
                              Finally, update the <code>fetch</code> event handler to handle requests to the data API separately from other requests.
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(e) {
  console.log('[Service Worker] Fetch', e.request.url);
  var dataUrl = 'https://query.yahooapis.com/v1/public/yql';
  if (e.request.url.indexOf(dataUrl) &gt; -1) {
    /*
     * When the request URL contains dataUrl, the app is asking for fresh
     * weather data. In this case, the service worker always goes to the
     * network and then caches the response. This is called the "Cache then
     * network" strategy:
     * https://jakearchibald.com/2014/offline-cookbook/#cache-then-network
     */
    e.respondWith(
      caches.open(dataCacheName).then(function(cache) {
        return fetch(e.request).then(function(response){
          cache.put(e.request.url, response.clone());
          return response;
        });
      })
    );
  } else {
    /*
     * The app is asking for app shell files. In this scenario the app uses the
     * "Cache, falling back to the network" offline strategy:
     * https://jakearchibald.com/2014/offline-cookbook/#cache-falling-back-to-network
     */
    e.respondWith(
      caches.match(e.request).then(function(response) {
        return response || fetch(e.request);
      })
    );
  }
});
</code></pre>
                            
                            <p>
                              The code intercepts the request and checks if the URL starts with the address of the weather API. If it does we'll use <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API"><code>fetch</code></a> to make the request. Once the response is returned, our code opens the cache, clones the response, stores it in the cache, and finally returns the response to the original requestor.
                            </p>
                            
                            <p>
                              Our app won't work offline quite yet. We've implemented caching and retrieval for the app shell, but even though we're caching the data, the app doesn't yet check the cache to see if it has any weather data.
                            </p>
                            
                            <h3>
                              Making the requests
                            </h3>
                            
                            <p>
                              As mentioned previously, the app needs to kick off two asynchronous requests, one to the cache and one to the network. The app uses the <code>caches</code> object available in <code>window</code> to access the cache and retrieve the latest data. This is an excellent example of progressive enhancement as the <code>caches</code> object may not be available in all browsers, and if it's not the network request should still work.
                            </p>
                            
                            <p>
                              To do this, we need to:
                            </p>
                            
                            <ol start="1">
                              <li>
                                Service Worker のコードを提供する JavaScript ファイルを作成します。
                              </li>
                              
                              <li>
                                作成した JavaScript ファイルを Service Worker として登録するようブラウザに指定します。
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                初回実行時には、<code>initialWeatherForecast</code> からの予報が即座に表示される必要があります。
                              </li>
                            </ul>
                            
                            <ol start="3">
                              <li>
                                グローバルな <code>window</code> オブジェクトにおいて、<code>caches</code> オブジェクトが利用可能か どうかを確認します。
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                Service Worker の登録が解除された後も、関連するブラウザ ウィンドウが閉じられる まで、Service Worker が表示されることがあります。
                              </li>
                              <li>
                                アプリに対して複数のウィンドウが開いている場合、新しい Service Worker の動作が 有効になるのは、すべてのウィンドウが再読み込みされて最新の Service Worker に更新されてからとなります。
                              </li>
                            </ul>
                            
                            <h4>
                              Get data from the cache
                            </h4>
                            
                            <p>
                              Next, we need to check if the <code>caches</code> object exists and request the latest data from it. Find the <code>TODO add cache logic here</code> comment in <code>app.getForecast()</code>, and then add the code below under the comment.
                            </p>
                            
                            <pre><code>    if ('caches' in window) {
      /*
       * Check if the service worker has already cached this city's weather
       * data. If the service worker has the data, then display the cached
       * data while the app fetches the latest data.
       */
      caches.match(url).then(function(response) {
        if (response) {
          response.json().then(function updateFromCache(json) {
            var results = json.query.results;
            results.key = key;
            results.label = label;
            results.created = json.query.created;
            app.updateForecastCard(results);
          });
        }
      });
    }
</code></pre>
                            
                            <p>
                              Our weather app now makes two asynchronous requests for data, one from the <code>cache</code> and one via an XHR. If there's data in the cache, it'll be returned and rendered extremely quickly (tens of milliseconds) and update the card only if the XHR is still outstanding. Then, when the XHR responds, the card will be updated with the freshest data direct from the weather API.
                            </p>
                            
                            <p>
                              Notice how the cache request and the XHR request both end with a call to update the forecast card. How does the app know whether it's displaying the latest data? This is handled in the following code from <code>app.updateForecastCard</code>:
                            </p>
                            
                            <pre><code>    var cardLastUpdatedElem = card.querySelector('.card-last-updated');
    var cardLastUpdated = cardLastUpdatedElem.textContent;
    if (cardLastUpdated) {
      cardLastUpdated = new Date(cardLastUpdated);
      // Bail if the card has more recent data then the data
      if (dataLastUpdated.getTime() &lt; cardLastUpdated.getTime()) {
        return;
      }
    }
</code></pre>
                            
                            <p>
                              Every time that a card is updated, the app stores the timestamp of the data on a hidden attribute on the card. The app just bails if the timestamp that already exists on the card is newer than the data that was passed to the function.
                            </p>
                            
                            <h3>
                              Test it out
                            </h3>
                            
                            <p>
                              The app should be completely offline-functional now. Save a couple of cities and press the refresh button on the app to get fresh weather data, and then go offline and reload the page.
                            </p>
                            
                            <p>
                              Then go to the <strong>Cache Storage</strong> pane on the <strong>Application</strong> panel of DevTools. Expand the section and you should see the name of your app shell and data cache listed on the left-hand side. Opening the data cache should should the data stored for each city.
                            </p>
                            
                            <p>
                              <img src="img/cf095c2153306fa7.png" alt="cf095c2153306fa7.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-07/">TRY IT</a>
                            </p>
                            
                            <h2>
                              Service Worker を使って App Shell を事前キャッシュする
                            </h2>
                            
                            <p>
                              Nobody likes to have to type in long URLs on a mobile keyboard if they don't need to. With the Add To home screen feature, your users can choose to add a shortcut link to their device just as they would install a native app from a store, but with a lot less friction.
                            </p>
                            
                            <h3>
                              Web App Install Banners and Add to Homescreen for Chrome on Android
                            </h3>
                            
                            <p>
                              Web app install banners give you the ability to let your users quickly and seamlessly add your web app to their home screen, making it easy to launch and return to your app. Adding app install banners is easy, and Chrome handles most of the heavy lifting for you. We simply need to include a web app manifest file with details about the app.
                            </p>
                            
                            <p>
                              Chrome then uses a set of criteria including the use of a service worker, SSL status and visit frequency heuristics to determine when to show the banner. In addition a user can manually add it via the "Add to Home Screen" menu button in Chrome.
                            </p>
                            
                            <h4>
                              Declare an app manifest with a <code>manifest.json</code> file
                            </h4>
                            
                            <p>
                              The web app manifest is a simple JSON file that gives you, the developer, the ability to control how your app appears to the user in the areas that they would expect to see apps (for example the mobile home screen), direct what the user can launch and more importantly how they can launch it.
                            </p>
                            
                            <p>
                              Using the web app manifest, your web app can:
                            </p>
                            
                            <ul>
                              <li>
                                Chrome DevTools を開き、Service Worker が適切に登録され正しいリソースが キャッシュされていることを確認します。
                              </li>
                              <li>
                                <code>cacheName</code> を変更してみて、キャッシュが適切に更新されることを確認します。
                              </li>
                              <li>
                                Control the screen orientation for optimal viewing
                              </li>
                              <li>
                                Define a "splash screen" launch experience and theme color for the site
                              </li>
                              <li>
                                Track whether you're launched from the home screen or URL bar
                              </li>
                            </ul>
                            
                            <p>
                              Create a file named <code>manifest.json</code> in your <code>work</code> folder and copy/paste the following contents:
                            </p>
                            
                            <pre><code>{
  "name": "Weather",
  "short_name": "Weather",
  "icons": [{
    "src": "images/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    }],
  "start_url": "/index.html",
  "display": "standalone",
  "background_color": "#3E4EB8",
  "theme_color": "#2F3BA2"
}
</code></pre>
                            
                            <p>
                              The manifest supports an array of icons, intended for different screen sizes. At the time of this writing, Chrome and Opera Mobile, the only browsers that support web app manifests, won't use anything smaller than 192px.
                            </p>
                            
                            <p>
                              An easy way to track how the app is launched is to add a query string to the <code>start_url</code> parameter and then use an analytics suite to track the query string. If you use this method, remember to update the list of files cached by the App Shell to ensure that the file with the query string is cached.
                            </p>
                            
                            <h4>
                              Tell the browser about your manifest file
                            </h4>
                            
                            <p>
                              Now add the following line to the bottom of the <code>&lt;head&gt;</code> element in your <code>index.html</code> file:
                            </p>
                            
                            <pre><code>&lt;link rel="manifest" href="/manifest.json"&gt;
</code></pre>
                            
                            <h4>
                              Best Practices
                            </h4>
                            
                            <ul>
                              <li>
                                コンソールで、更新のたびに 2 つのイベント（キャッシュからデータが取得されたことを 示すイベントと、ネットワークからデータが取得されたことを示すイベント）が表示される ことを確認します。
                              </li>
                              <li>
                                この時点で、アプリは完全にオフラインで動作するようになっています。開発用のサーバー を停止し、ネットワークの接続を切断して、アプリを実行してみてください。App Shell とデータの両方がキャッシュから配信されるようになります。
                              </li>
                              <li>
                                Define icon sets for different density screens. Chrome will attempt to use the icon closest to 48dp, for example, 96px on a 2x device or 144px for a 3x device.
                              </li>
                              <li>
                                Remember to include an icon with a size that is sensible for a splash screen and don't forget to set the <code>background_color</code>.
                              </li>
                            </ul>
                            
                            <p>
                              Further Reading:
                            </p>
                            
                            <p>
                              <a href="/web/fundamentals/engage-and-retain/simplified-app-installs/">Using app install banners</a>
                            </p>
                            
                            <h3>
                              Add to Homescreen elements for Safari on iOS
                            </h3>
                            
                            <p>
                              In your <code>index.html</code>, add the following to the bottom of the <code>&lt;head&gt;</code> element:
                            </p>
                            
                            <pre><code>  &lt;!-- Add to home screen for Safari on iOS --&gt;
  &lt;meta name="apple-mobile-web-app-capable" content="yes"&gt;
  &lt;meta name="apple-mobile-web-app-status-bar-style" content="black"&gt;
  &lt;meta name="apple-mobile-web-app-title" content="Weather PWA"&gt;
  &lt;link rel="apple-touch-icon" href="images/icons/icon-152x152.png"&gt;
</code></pre>
                            
                            <h3>
                              Tile Icon for Windows
                            </h3>
                            
                            <p>
                              In your <code>index.html</code>, add the following to the bottom of the <code>&lt;head&gt;</code> element:
                            </p>
                            
                            <pre><code>  &lt;meta name="msapplication-TileImage" content="images/icons/icon-144x144.png"&gt;
  &lt;meta name="msapplication-TileColor" content="#2F3BA2"&gt;
</code></pre>
                            
                            <h3>
                              Test it out
                            </h3>
                            
                            <p>
                              In this section we'll show you a couple of ways to test your web app manifest.
                            </p>
                            
                            <p>
                              The first way is DevTools. Open up the <strong>Manifest</strong> pane on the <strong>Application</strong> panel. If you've added the manifest information correctly, you'll be able to see it parsed and displayed in a human-friendly format on this pane.
                            </p>
                            
                            <p>
                              You can also test the add to homescreen feature from this pane. Click on the <strong>Add to homescreen</strong> button. You should see a "add this site to your shelf" message below your URL bar, like in the screenshot below.
                            </p>
                            
                            <p>
                              <img src="img/cbfdd0302b611ab0.png" alt="cbfdd0302b611ab0.png" />
                            </p>
                            
                            <p>
                              This is the desktop equivalent of mobile's add to homescreen feature. If you can successfully trigger this prompt on desktop, then you can be assured that mobile users can add your app to their devices.
                            </p>
                            
                            <p>
                              The second way to test is via Web Server for Chrome. With this approach, you expose your local development server (on your desktop or laptop) to other computers, and then you just access your progressive web app from a real mobile device.
                            </p><aside class="warning">

<p>Opening up a port for remote access is handy for testing this step but may be blocked by your computer's firewall rules or network administrator. Opening ports for remote access is generally not a good thing to leave running on your computer. So, for security reasons, when you've completed testing this step, disable the <code>Accessible on local network</code> option and restart your web server.</p>

</aside> 
                            
                            <p>
                              On Web Server for Chrome configuration dialog, select the <code>Accessible on local network</code> option:
                            </p>
                            
                            <p>
                              <img src="img/81347b12f83e4291.png" alt="81347b12f83e4291.png" />
                            </p>
                            
                            <p>
                              Toggle the Web Server to <code>STOPPED</code> and back to <code>STARTED</code>. You'll see a new URL which can be used to access your app remotely.
                            </p>
                            
                            <p>
                              Now, access your site from a mobile device, using the new URL.
                            </p>
                            
                            <p>
                              You will see service worker errors in the console when testing this way because the service worker is not being served over HTTPS.
                            </p>
                            
                            <p>
                              Using Chrome from an Android device, try adding the app to the homescreen and verifying that the launch screen appears properly and the right icons are used.
                            </p>
                            
                            <p>
                              On Safari and Internet Explorer, you can also manually add the app to your homescreen.
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-08/">TRY IT</a>
                            </p>
                            
                            <h2>
                              Service Worker を使ってアプリケーション データをキャッシュする
                            </h2>
                            
                            <p>
                              The final step is to deploy our weather app to a server that supports HTTPS. If you don't already have one, the absolute easiest (and free) approach is to use the static content hosting from Firebase. It's super easy to use, serves content over HTTPS and is backed by a global CDN.
                            </p>
                            
                            <h3>
                              Extra credit: minify and inline CSS
                            </h3>
                            
                            <p>
                              There's one more thing that you should consider, minifying the key styles and inlining them directly into <code>index.html</code>. <a href="/speed">Page Speed Insights</a> recommends serving the above the fold content in the first 15k bytes of the request.
                            </p>
                            
                            <p>
                              See how small you can get the initial request with everything inlined.
                            </p>
                            
                            <p>
                              Further Reading: <a href="/speed/docs/insights/rules">PageSpeed Insight Rules</a>
                            </p><aside class="key-point">

<p>This step requires you to have  <a href="https://docs.npmjs.com/getting-started/installing-node">Node &#x26; NPM</a> installed on your system. If it's not, you can use any other hosting provider that supports HTTP<strong>S</strong>. We've used Firebase because it automatically redirects users from HTTP to HTTP<strong>S</strong>. If you use a different provider, be sure they're always redirects to HTTP<strong>S</strong>.</p>

</aside> 
                            
                            <h3>
                              Deploy to Firebase
                            </h3>
                            
                            <p>
                              If you're new to Firebase, you'll need to create your account and install some tools first.
                            </p>
                            
                            <ol start="1">
                              <li>
                                Create a Firebase account at <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a>
                              </li>
                              
                              <li>
                                Install the Firebase tools via npm: <code>npm install -g firebase-tools</code>
                              </li>
                            </ol>
                            
                            <p>
                              Once your account has been created and you've signed in, you're ready to deploy!
                            </p>
                            
                            <ol start="1">
                              <li>
                                Create a new app at <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a>
                              </li>
                              
                              <li>
                                If you haven't recently signed in to the Firebase tools, update your credentials: <code>firebase login</code>
                              </li>
                              
                              <li>
                                Initialize your app, and provide the directory (likely <code>work</code>) where your completed app lives: <code>firebase init</code>
                              </li>
                              
                              <li>
                                Finally, deploy the app to Firebase: <code>firebase deploy</code>
                              </li>
                              
                              <li>
                                Celebrate. You're done! Your app will be deployed to the domain: <code>https://YOUR-FIREBASE-APP.firebaseapp.com</code>
                              </li>
                            </ol>
                            
                            <p>
                              Further reading: <a href="https://firebase.google.com/docs/hosting/">Firebase Hosting Guide</a>
                            </p>
                            
                            <h3>
                              Test it out
                            </h3>
                            
                            <ul>
                              <li>
                                Try adding the app to your home screen then disconnect the network and verify the app works offline as expected.
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/final/">TRY IT</a>
                            </p>
                            
                            <h2>
                              Found an issue, or have feedback? {: .hide-from-toc }
                            </h2>
                            
                            <p>
                              Help us make our code labs better by submitting an <a href="https://github.com/googlecodelabs/your-first-pwapp/issues">issue</a> today. And thanks!
                            </p>