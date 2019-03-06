project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: セキュリティは重要なトピックです。HTTPS が重要な理由と HTTPS をサーバーにデプロイする方法について説明します。

{# wf_updated_on:2016-09-09 #} {# wf_published_on:2015-09-08 #}

# セキュリティとアイデンティティ {: .page-title }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="pgBQn_z3zRE"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

セキュリティは重要なトピックです。まずはこのページで概要をご確認ください。

<div class="clearfix"></div>

## 送信中のデータの暗号化

<img src="/web/images/content-https-2x.jpg" class="attempt-right" />

多くの最新の API と [Progressive Web App](/web/progressive-web-apps/) に必要な最も重要なセキュリティ機能の 1 つは、[セキュアな HTTP（HTTPS）](encrypt-in-transit/why-https)です。HTTPS が必要なのは、機密性の高い情報を通信するウェブサイトのみであると一般的に思われていますが、これは誤りです。プライバシーとセキュリティの観点からユーザーを保護することに加えて、Service Worker や Payment Request API などの多くの新しいブラウザ機能で HTTPS が必須になっています。

Some people mistakenly believe that the only sites that need HTTPS are sites that handle some level of sensitive communication, like personal or financial data. But this isn't true. Every site should be using HTTPS, HTTPS helps to prevents people from listening into what's crossing the wire, and helps prevent it from being tampered with while in transit. Do you want your ISP or school to know every site you were looking at?

{# wf_devsite_translation #}

[Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https)

<div class="attempt-left">
  <h2>コンテンツ セキュリティ ポリシー</h2>
  <p>
    コンテンツ セキュリティ ポリシー（CSP）でサポートされる種類豊富なディレクティブを利用すると、ページへの読み込みが可能なリソースおよびページの読み込み元のリソースをより細かく制御できるようになります。<br>


    <a href="csp/">詳細を見る</a>
  </p>
</div>

<div class="attempt-right">
  <h2>
    混合コンテンツの防止
  </h2>
  
  <p>
    HTTPS の実装で最も時間のかかる作業の 1 つは、HTTPS と HTTP が混在するコンテンツを見つけて修正することです。 幸いにも、この作業に役立つツールがいくつかあります。<br /> <a href="prevent-mixed-content/what-is-mixed-content">スタートガイド</a>
  </p>
</div>

<div style="clear:both"></div>

## 関連リソース

* [セキュリティの問題を理解する](https://www.youtube.com/watch?v=tgEIo7ZSkbQ)
* [Getting the Green Lock: HTTPS Stories from the Field](https://www.youtube.com/watch?v=GoXgl9r0Kjk)

### Chrome DevTools

* [Understand Security Issues](/web/tools/chrome-devtools/security)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}