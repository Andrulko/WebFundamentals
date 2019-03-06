project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: アクセシビリティ ツリーの概要

{# wf_updated_on:2016-10-04 #} {# wf_published_on:2016-10-04 #}

# アクセシビリティ ツリー {: .page-title }

{% include "web/_shared/contributors/megginkearney.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/aliceboxhall.html" %}

ここでは、*スクリーン リーダーのユーザー専用*のユーザー インターフェースを作成するものとします。視覚的な UI を作成する必要はまったくありませんが、スクリーン リーダーで使用するために十分な情報を提供します。

ここで作成するのは、DOM API と同様にページの構成を示す一種の API ですが、情報の多くは視覚表示にのみ効果があるため、情報もノードも少なくて済みます。次のようになります。

![スクリーン リーダー DOM API モックアップ](imgs/treestructure.jpg)

これは基本的に、ブラウザが実際にスクリーン リーダーに提示する内容です。ブラウザは DOM ツリーを取得して、支援技術に使いやすい形式に変更します。この変更したツリーを*アクセシビリティ ツリー*と呼びます。

このアクセシビリティ ツリーは、たくさんのリンクがあり、画像が少なく、フィールドやボタンが含まれ、90 年代の古いウェブページのようなイメージです。

![1990 年代スタイルのウェブページ](imgs/google1998.png)

このようなページを下に向かって見ていくと、スクリーン リーダーのユーザーと同様のエクスペリエンスが得られます。インターフェースはありますが、シンプルかつ直接的で、アクセシビリティ ツリーのインターフェースに非常によく似ています。

アクセシビリティ ツリーは、ほとんどの支援技術で利用されています。フローは次のようになります。

1. An application (the browser or other app) exposes a semantic version of its UI to assistive technology via an API.
2. The assistive technology may use the information it reads via the API to create an alternative user interface presentation for the user. For example, a screen reader creates an interface in which the user hears a spoken representation of the app.
3. The assistive technology may also allow the user to interact with the app in a different way. For example, most screen readers provide hooks to allow a user to easily simulate a mouse click or finger tap.
4. The assistive technology relays the user intent (such as "click") back to the app via the accessibility API. The app then has the responsibility to interpret the action appropriately in the context of the original UI.

ウェブブラウザの場合、指示ごとに追加のステップがあります。ブラウザは実際のところ、ブラウザ内部で実行するウェブアプリのプラットフォームだからです。そのためブラウザは、ウェブアプリをアクセシビリティ ツリーに変換し、支援技術から受け取ったユーザーのアクションに基づいて、JavaScript で適切なイベントが発行されるようにする必要があります。

しかし、ブラウザが行う処理はそれだけです。ウェブ デベロッパーとしての役割は、このような流れを認識したうえで、このプロセスを活用できるウェブページを開発し、ユーザーがアクセスできる環境を整えることです。

そのためには、ページのセマンティクスを正しく表現して、ページ内の重要な要素に適切な役割、状態、プロパティが存在し、アクセス可能な名前と説明を指定していることを確認する必要があります。そうすればブラウザ側で、支援技術がその情報にアクセスして、カスタマイズされたエクスペリエンスを作成できるようにすることが可能です。

## ネイティブ HTML のセマンティクス

DOM の多くはセマンティックな意味を*暗黙的に*示しているため、ブラウザは、DOM ツリーをアクセシビリティ ツリーに変換できます。つまり、DOM は、ブラウザによって認識され、さまざまなプラットフォームで予測可能な形で動作するネイティブ HTML 要素を使用します。よって、リンクやボタンといったネイティブ HTML 要素のアクセシビリティは、自動的に処理されます。この組み込みのアクセシビリティを活用するには、ページ要素のセマンティクスを表す HTML を記述します。

ただし、ネイティブ要素のように見えるけれども実際は異なる要素を使用することもあります。たとえば、この「ボタン」は実際にはボタンではありません。

{% framebox height="60px" %}

<style>
    .fancy-btn {
        display: inline-block;
        background: #BEF400;
        border-radius: 8px;
        padding: 10px;
        font-weight: bold;
        user-select: none;
        cursor: pointer;
    }
</style>

<div class="fancy-btn">Give me tacos</div>

{% endframebox %}

これを HTML で作成する方法は数多くありますが、一例を示します。

    <div class="button-ish">Give me tacos</div>
    

実際のボタン要素を使用しないときは、それがどのような要素なのか、スクリーン リーダーは知る術がありません。また、[tabindex を追加](/web/fundamentals/accessibility/focus/using-tabindex)して、キーボードのみで操作するユーザーにも使用可能にするという追加の作業が必要になります。現状のコードのままでは、マウスがなければ使用できないからです。`div` の代わりに通常の `button` 要素を使用すれば、この問題は簡単に解決できます。ネイティブ要素を使用すれば、キーボード操作に対応できるというメリットもあります。ネイティブ要素を使用するからといって、魅力的な視覚効果を諦める必要はありません。ネイティブ要素のスタイルを指定して目的の外観を実現し、しかも暗黙的なセマンティクスと動作を維持することができます。

先ほど説明したとおり、スクリーン リーダーは要素の役割、名前、状態、値を通知します。 適切なセマンティクス要素、役割、状態、値の使用については説明しましたが、さらに要素の名前を検出可能にする必要があります。

大まかに、名前には以下の 2 つのタイプがあります。

テキストレベルの要素の場合、なにもする必要はありません。定義すれば、なんらかのテキストのコンテンツがあるからです。しかし、入力要素やコントロール要素、画像のような視覚的なコンテンツの場合、名前を指定していることを確認する必要があります。実際、テキスト以外のコンテンツに代替テキストを指定することは、[WebAIM チェックリストの最初の項目に挙げられています](http://webaim.org/standards/wcag/checklist#g1.1)。

- *Visible labels*, which are used by all users to associate meaning with an element, and
- *Text alternatives*, which are only used when there is no need for a visual label.

その方法の 1 つは、推奨案に従い「フォームの入力要素に、関連するテキストのラベルを付ける」ことです。ラベルと、チェックボックスなどのフォーム要素を関連付ける方法は 2 つあります。いずれかの方法で、ラベルテキストをチェックボックスのクリック ターゲットにすると、マウスのユーザーにもタッチスクリーンのユーザーにもメリットがあります。ラベルと要素を関連付けるには、次のいずれかを実行します。

{% framebox height="60px" %}

- ラベル要素内に入力要素を配置する

<div class="clearfix"></div>

    <label>
      <input type="checkbox">プロモーション情報を受け取る</input>
    </label>
    

{% endframebox %}

<div style="margin: 10px;">
    <label style="font-size: 16px; color: #212121;">
        <input type="checkbox">プロモーション情報を受け取る</input>
    </label>
</div>

インテントまたは

{% framebox height="60px" %}

- ラベルの `for` 属性を使用して、要素の `id` を参照する

<div class="clearfix"></div>

    <input id="promo" type="checkbox"></input>
    <label for="promo">プロモーション情報を受け取る</label>
    

{% endframebox %}

<div style="margin: 10px;">
    <input id="promo" type="checkbox"></input>
    <label for="promo">プロモーション情報を受け取る</label>
</div>

チェックボックスのラベルを適切に設定すると、スクリーン リーダーは要素にチェックボックスの役割があり、オンの状態で、「プロモーション情報を受け取る」という名前が付いていることを報告できます。

When the checkbox has been labeled correctly, the screen reader can report that the element has a role of checkbox, is in a checked state, and is named "Receive promotional offers?".

![on-screen text output from VoiceOver showing the spoken label for a checkbox](imgs/promo-offers.png)

{# wf_devsite_translation #}

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}