project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: クリティカル レンダリング パスにおけるパフォーマンスのボトルネックの特定および解消法について説明します。

{# wf_updated_on:2014-04-27 #} {# wf_published_on:2014-03-31 #}

# クリティカル レンダリング パスのパフォーマンスを分析する {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

クリティカル レンダリング パスにおけるパフォーマンスのボトルネックを特定して解消するには、注意が必要なポイントを把握しておく必要があります。 このページでは、実践的な例を取り上げながら、ページの最適化に役立つ一般的なパフォーマンス パターンについて紹介します。

クリティカル レンダリング パスを最適化する目的は、できる限り早くブラウザがページを描画できるようにすることです。ページの高速化は、エンゲージメントの向上、ページの閲覧回数の増加、[コンバージョン率の改善](https://www.google.com/think/multiscreen/success.html)につながります。訪問者が何もない画面を見つめるだけの時間を最小限にするため、「どのリソースのどの順で読み込むか」を最適化することが必要です。

このプロセスを説明するために、まずは最もシンプルなケースから始めて、徐々にリソースやスタイル、アプリケーション ロジックを追加してページを構築していきます。その過程で、ケースごとの最適化を行い、失敗しやすいポイントついても説明します。

これまでは、リソース（CSS、JavaScript、HTML などのファイル）が処理できる状態になったあと、ブラウザ側で行われる処理だけに焦点を当てており、リソースをキャッシュから取得する場合とネットワークから取得する場合の所要時間については考慮していませんでした。ここでは、次の前提条件があるとします。

* サーバーまでのネットワーク ラウンドトリップ（プロパゲーション レイテンシ）は 100 ms
* サーバーの応答時間は、HTML ドキュメントの場合は 100 ms、その他のファイルの場合は 10 ms

## Hello World サンプル

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[サンプルを見る](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

まずは CSS と JavaScript は使わずに、基本的な HTML マークアップと 1 つの画像から始めましょう。Chrome DevTools でネットワーク タイムラインを開き、リソース ウォーターフォールを確認します。

<img src="images/waterfall-dom.png" alt="CRP" />

注: このドキュメントでは DevTools を使用して CRP のコンセプトを説明しますが、現在のところ、DevTools は CRP 分析にあまり適してはいません。 詳細については、[DevTools に関するドキュメント](measure-crp#devtools)をご覧ください。

想定どおり、HTML のダウンロードに約 200 ms かかっています。青色の線の透過部分は、ブラウザが応答バイトを受け取っておらず、ネットワーク上で待機している時間を表します。一方、塗りつぶされた部分は、最初の応答バイトを受け取ってからダウンロードが完了するまでの時間を表します。HTML のダウンロード量はわずか（4 K 未満）であるため、1 回のラウンドトリップでファイル全体を取得できます。そのため、HTML ドキュメントを取得するための所要時間は約 200 ms です。この時間の半分はネットワーク上で待機しており、残りの半分はサーバーの応答を待っています。<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

HTML コンテンツが利用可能になると、ブラウザはバイトを解析してトークンに変換し、DOM ツリーを構築する必要があります。DevTools の下の方には、便宜のために、DOMContentLoaded イベントの時間（216 ms）が表示されています。これは、青色の縦線に相当します。HTML ダウンロードの完了時点と青色の縦線（DOMContentLoaded）の差が、ブラウザで DOM ツリーを構築するのに要した時間です。今回の場合、この時間は数ミリ秒にすぎません。

「awesome photo」が `domContentLoaded` イベントをブロックしていない点にも注目してください。これは、ページの各アセットを待たずに、レンダリング ツリーの構築やページのレンダリングができることを示しています。**初回のレンダリングを高速化する上で、すべてのリソースが必須というわけではありません**。後で、クリティカル レンダリング パスに関するトピックで説明するように、一般に検討対象となるのは、HTML マークアップ、CSS、JavaScript です。画像は、初回のページ レンダリングをブロックしませんが、できる限り早く画像がレンダリングされるように配慮する必要はあります。

ただし、`load` イベント（`onload`）は、画像によってブロックされます。DevTools では、335 ms で `onload` イベントが記録されています。前に説明したとおり、`onload` イベントは、ページに必要な**すべてのリソース**がダウンロードされ、処理が完了した時点を表します。この段階で、ブラウザの読み込み中マークの回転が止まります（ウォーターフォール上の赤色の縦線に相当）。

## JavaScript と CSS をサンプルに追加する

「Hello World サンプル」ページは、一見するとシンプルに見えますが、内部ではさまざまな処理が実行されていました。また、現実的には HTML 以外の要素も必要になります。CSS スタイルシートと 1 つ以上のスクリプトを組み合わせて、インタラクティブなページにするケースも多くあります。この両者をサンプルに追加して、どうなるか見てみましょう。

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_timing.html){: target="_blank" .external }

*Before adding JavaScript and CSS:*

<img src="images/waterfall-dom.png" alt="DOM CRP" />

*With JavaScript and CSS:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

何が起きたのでしょう？

**外部スクリプトをインライン スクリプトに置き換えるとどうなるでしょうか。** スクリプトをインラインでページに直接組み込んだとしても、CSSOM が構築されるまで、ブラウザはスクリプトを実行できません。つまり、インライン JavaScript もパーサー ブロックになります。

* 前述の HTML のみのサンプルとは異なり、今回は CSS ファイルを取得して解析し、CSSOM を構築する必要があります。また、レンダリング ツリーの構築には、DOM と CSSOM の両方が必要です。
* パーサーをブロックする JavaScript ファイルをページに追加したことで、CSS ファイルのダウンロードと解析が完了するまで、`domContentLoaded` イベントがブロックされています。この理由は、JavaScript が CSSOM に対してクエリを実行する場合があるため、JavaScript を実行する前に、CSS ファイルをブロックしてダウンロードを待つ必要があるためです。

CSS をブロックしても、インライン スクリプトの方がページのレンダリングが高速になるでしょうか。実際に試してみましょう。

That said, despite blocking on CSS, does inlining the script make the page render faster? Let's try it and see what happens.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Inlined JavaScript:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, and inlined JS" />

まず、前述したように、インライン スクリプトは常にパーサー ブロックですが、外部スクリプトは、async キーワードを追加してパーサー ブロックではなくすことができます。インライン化を元に戻し、試してみましょう。

[サンプルを見る](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

*非同期（外部）JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM、CSSOM、非同期 JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

[サンプルを見る](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_inlined.html){: target="_blank" .external }

Alternatively, we could have inlined both the CSS and JavaScript:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

`domContentLoaded` のタイミングは、前のサンプルとほぼ変わりません。今回は JavaScript を非同期にする代わりに、CSS と JavaScript の両方を直接ページにインライン化しています。これにより、HTML ページのサイズはかなり大きくなっていますが、すべてがページ内にあるため、ブラウザで外部リソースの取得を待つ必要がないというメリットがあります。

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" />

では、これらのことを踏まえて、一般的なパフォーマンス パターンを特定していきましょう。

最もシンプルなページは、CSS、JavaScript、その他のリソースを含まず、HTML マークアップだけで構成されているページです。このページをレンダリングするために、ブラウザはリクエストを開始し、HTML ドキュメントが届くのを待ち、それを解析し、DOM を構築して、ようやく画面上にレンダリングします。

[サンプルを見る](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

## パフォーマンス パターン

The simplest possible page consists of just the HTML markup; no CSS, no JavaScript, or other types of resources. To render this page the browser has to initiate the request, wait for the HTML document to arrive, parse it, build the DOM, and then finally render it on the screen:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**T<sub>0</sub> と T<sub>1</sub> の間の時間は、ネットワークとサーバーの処理時間を表します。**ベストケースの場合（HTML ファイルが小さい場合）、1 回 のネットワーク ラウンドトリップでドキュメント全体を取得できます。ファイルが大きい場合は、TCP 転送プロトコルの仕組み上、必要なラウンドトリップ数が増える可能性があります。**つまり、ベストケースにおいては、上記のページのクリティカル レンダリング パスは（最低で）1 回のラウンドトリップになります。**

<img src="images/analysis-dom.png" alt="Hello world CRP" />

[サンプルを見る](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

Now, let's consider the same page but with an external CSS file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

繰り返しになりますが、HTML ドキュメントを取得する際はネットワーク ラウンドトリップが発生し、取得したマークアップから CSS ファイルも必要であることがわかります。つまり、ブラウザは画面上にページをレンダリングするために、サーバーに戻って CSS を取得する必要があります。**結果的に、このページを表示するには最低 2 回のラウンドトリップが発生します。**CSS ファイルの取得に複数回のラウンドトリップが必要になる場合があるので、「最低」で 2 回です。

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

では、これと上記の HTML + CSS のサンプルにおけるクリティカル パスの特徴を比較してみましょう。

Let's define the vocabulary we use to describe the critical rendering path:

* ** クリティカル リソース:** 初回のページ レンダリングをブロックする可能性のあるリソース。
* ** クリティカル パス長:** すべてのクリティカル リソースを取得するために必要なラウンドトリップ数または総時間。
* ** クリティカル バイト:** 初回のページ レンダリングに必要な合計バイト数。これは、すべてのクリティカル リソースの転送ファイルサイズの合計になります。最初のサンプルは 1 つのクリティカル リソース（HTML ドキュメント）を含む単一の HTML ページなので、クリティカル パス長は 1 ネットワーク ラウンドトリップ（ファイルサイズが小さい場合）、総クリティカル バイトは HTML ドキュメント自体の転送サイズだけになります。

レンダリング ツリーの構築には、HTML と CSS の両方が必要です。そのため、HTML と CSS の両方がクリティカル リソースとなります。CSS は、ブラウザが HTML ドキュメントを取得した後にのみ取得可能になるため、クリティカル パス長は、最低で 2 ラウンドトリップとなります。クリティカル バイトは合計 9 KB です。

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* クリティカル リソースの数は **2** つ
* 最小クリティカル パス長のラウンド トリップ数は **2** 以上
* クリティカル バイト数は **9** KB

[サンプルを見る](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

ページの外部 JavaScript アセットとして `app.js` を追加しました。これはパーサー ブロック リソースであり、クリティカル リソースになります。さらに、JavaScript ファイルを実行するには、ブロックして CSSOM を待つ必要があります。JavaScript では CSSOM に対するクエリが可能であるため、ブラウザは、`style.css` がダウンロードされて CSSOM が構築されるまで、一時停止します。

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

ところで、このページの「ネットワーク ウォーターフォール」を確認すると、CSS リクエストと JavaScript リクエストがほぼ同じタイミングで開始されていることがわかります。ブラウザは HTML を取得し、両方のリソースを発見して両方のリクエストを開始しています。そのため、上記ページのクリティカル パスの特徴は、次のようになります。

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

サイト デベロッパーからの情報で、ページに組み込まれている JavaScript はブロック不要であることがわかりました。スクリプトに含まれるアナリティクスや他のコードは、ページのレンダリングをブロックする必要がありません。よって、script タグに「async」属性を追加して、パーサーをブロックしないようにすることができます。

* クリティカル リソースの数は **3** つ
* 最小クリティカル パス長のラウンド トリップ数は **2** 以上
* クリティカル バイト数は **11** KB

[サンプルを見る](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

非同期スクリプトにはいくつかメリットがあります。

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

最後に、CSS スタイルシートが印刷にのみ必要なケースではどうなるのか見てみましょう。

* スクリプトがパーサー ブロックにならず、クリティカル レンダリング パスの一部ではなくなります。
* 他にクリティカル スクリプトがないため、CSS が `domContentLoaded` イベントをブロックする必要もなくなります。
* `domContentLoaded` イベントが早く発生するほど、他のアプリケーション ロジックも早く実行できるようになります。

[サンプルを見る](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

Finally, if the CSS stylesheet were only needed for print, how would that look?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

style.css リソースは印刷にのみ使用されるため、ブラウザでは、ページをレンダリングする際に CSS をブロックする必要がありません。したがって、DOM 構築が完了した時点で、ブラウザにはページのレンダリングに必要な情報がすべてそろっています。その結果、このページのクリティカル リソースは 1 つ（HTML ドキュメント）、最小クリティカル レンダリング パス長は 1 ラウンドトリップになります。

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

{# wf_devsite_translation #}

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}