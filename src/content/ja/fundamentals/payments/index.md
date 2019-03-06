project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Payment Request API は、ウェブでの迅速で簡単な支払を実現するためのものです。

{# wf_published_on:2016-07-25 #} {# wf_updated_on:2016-12-06 #}

# Payment Request API: 統合ガイド {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/zkoch.html" %}

<div class="video-wrapper-full-width">
  <iframe class="devsite-embedded-youtube-video" data-video-id="colCcgKoLUM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

## Payment Request API の導入 {: #introducing }

試験運用: `PaymentRequest` はまだ開発段階です。十分に安定しており、実装できる状態になっていると考えられますが、今後もまだ変更される可能性があります。 このページは、この API の最新ステータス（[M56 での変更点](https://docs.google.com/document/d/1I8ha1ySrPWhx80EB4CVPmThkD4ILFM017AfOA5gEFg4/edit#)）を反映するために引き続き更新されます。あわせて Google では、API の変更に伴う後方互換性の問題を軽減するために、サイトに埋め込み可能な [shim](https://storage.googleapis.com/prshim/v1/payment-shim.js) を提供しています。shim は、Chrome の 2 つのメジャー バージョン間の API の差異を埋めるためのものです。

オンラインでの商品購入は便利ですが、特にモバイル端末ではストレスを感じることがよくあります。モバイル トラフィックは増え続けているのに、モバイル コンバージョンは全購入件数の 3 分の 1 程度にすぎません。言い換えると、ユーザーはパソコンで購入する場合より 2 倍も多く、モバイルでの購入を取りやめています。なぜでしょうか。

**For consumers**, they simplify checkout flow, by making it a few taps instead of typing small characters many times on a virtual keyboard.

**For merchants**, they make it easier to implement with a variety of payment options already filtered for the customer.

オンライン購入フォームは、ユーザーによる処理が多く、使いづらく、読み込みと更新に時間がかかり、完了までに複数のステップが必要です。その理由として、オンライン決済の 2 つの主要素であるセキュリティと利便性が一般的に両立しないことが挙げられます。通常、一方を優先すれば、もう一方が手薄になります。

ユーザーが購入を取りやめることになる原因を突き止めると、ほとんどが購入フォームに行き着きます。アプリやサイトは、データ入力と検証にそれぞれ独自のプロセスを使用しています。ユーザーの多くが、すべてのアプリの購入ポイントで同じ情報を入力しなければならないと感じています。また、アプリケーション デベロッパーは、複数の異なる支払方法に対応する購入フローの作成に苦心しています。支払方法の要件がわずかに異なるだけでも、フォームの入力と送信プロセスが複雑になります。

## Payment Request API の使用 {: #using }

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/standard-open.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Standard and Open</h3>
    Web Payments are an open payment standard for the web platform for the first time
    in history. They are available for any players to implement.</div>
</section>

<section style="display:flex;padding-bottom:32px;">
  <div style="min-width:50%">
    <h3>Easy and Consistent</h3>
    Web Payments make checkout easy for the user, by reusing stored 
payments and address information and removing the need for the user to fill in checkout forms. 
Since the UI is implemented by the browser natively, users see a familiar and consistent checkout 
experience on any website that makes use of the standard.</div>
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/easy-consistent.png" width="100%" alt="Standard and Open" title="">
  </div>
</section>

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/secure-flexible.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Secure and Flexible</h3>
    Web Payments provide industry-leading payment technology to the 
web, and can easily integrate a secure payment solution.</div>
</section>

## 配送先住所の収集{: #shipping-address }

このような問題を 1 つ以上改善または解決できるシステムは、すべて歓迎すべき変更です。既に[自動入力](/web/updates/2015/06/checkout-faster-with-autofill)を使用した問題解決に着手しましたが、ここではより包括的な解決策を取り上げます。

Payment Request API は、*支払フォームをなくす*ことを目的としたシステムです。購入プロセスにおけるユーザー ワークフローを大幅に改善することで、ユーザー エクスペリエンスをより一貫性のあるものにし、ウェブ販売者が複数の異なる支払方法を簡単に利用できるようにします。Payment Request API は新しい支払方法でもなければ、決済サービスと直接統合されるものでもありません。これは、次のことを目的としたプロセス レイヤーです。

Payment Request API は、従来の支払フローに代わる、オープンなクロスブラウザ標準で、販売者があらゆる支払を単一の API 呼び出しでリクエストおよび許可できるようにするものです。Payment Request API を使用すると、ウェブページは、支払リクエストを承認または拒否する前に、ユーザーの入力中にユーザー エージェントと情報を交換することができます。

何よりも優れているのは、ブラウザが仲介として機能するため、迅速な支払に必要なすべての情報をブラウザに保存できることです。そのためユーザーは確認して支払うだけでよく、すべてをシングル クリックで行うことができます。

Payment Request API を使用することで、ユーザーと販売者の両方にとって取引プロセスを最大限にシームレスにすることができます。

## 発送オプションの追加 {: #shipping-options}

{% include "web/_shared/helpful.html" %}