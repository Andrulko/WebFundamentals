project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description:Payment Request API 用于在网上实现快速简便的支付。

{# wf_published_on:2016-07-25 #} {# wf_updated_on:2016-12-06 #}

# Payment Request API：集成指南 {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/zkoch.html" %}

<div class="video-wrapper-full-width">
  <iframe class="devsite-embedded-youtube-video" data-video-id="colCcgKoLUM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

## Payment Request API 简介 {: #introducing }

Dogfood：`PaymentRequest` 仍处于开发阶段。虽然我们认为其稳定性足以满足实现的要求，但可能仍需作出改动。 我们会持续更新本页，以时刻反映 API 的最新状况（[M56 变更](https://docs.google.com/document/d/1I8ha1ySrPWhx80EB4CVPmThkD4ILFM017AfOA5gEFg4/edit#)）。在此期间，为让您免于受到可能不具有向后兼容性的 API 变更的影响，我们提供了可嵌入网站的 [shim](https://storage.googleapis.com/prshim/v1/payment-shim.js)。这个 shim 可平息两个主流 Chrome 版本的任何 API 差异。

在线购物非常方便，但通常存在令人沮丧的体验，在移动设备上购物尤其如此。虽然移动流量在不断增长，但移动购物转化率仅占所有已完成购物活动的三分之一。换言之，移动设备用户的购物放弃率是桌面设备用户的两倍。为何？

**For consumers**, they simplify checkout flow, by making it a few taps instead of typing small characters many times on a virtual keyboard.

**For merchants**, they make it easier to implement with a variety of payment options already filtered for the customer.

在线购物单需要大量用户操作、难以使用，加载和刷新缓慢，并且需要多个步骤才能完成。这是因为在线支付的两大要素（即安全和便利）之间通常互为矛盾，左支右绌。

导致用户放弃交易的大部分问题都均与购物单有着直接关系。每个应用或网站均有自己的数据输入和验证流程，用户通常发现，在每个应用的购物点他们都需要输入相同的信息。此外，应用开发者力求创建支持多种独特支付方式的购物流程；即使是支付方式要求的细小差异也可能导致表单填写和提交过程变得相当复杂。

## 使用 Payment Request API{: #using }

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

## 收集收货地址 {: #shipping-address }

任何能或多或少改进这些问题的系统都值得提倡。我们已经尝试借助[自动填充](/web/updates/2015/06/checkout-faster-with-autofill)功能来帮助解决问题，但我们现在有了更全面的解决方案。

Payment Request API 是一个旨在*消灭结账表单*的系统。它显著改进了购物流程期间的用户工作流，为用户提供更一致的体验，并让电商公司能够轻松地利用各种完全不同的支付方式。Payment Request API 不是一种新的支付方式，也不与支付处理机构直接整合；它是一种旨在实现以下目标的处理层：

Payment Request API 是一种开放式的跨浏览器标准，可以取代传统的结账流程，让商家能够在单个 API 调用中请求和接受任何付款。Payment Request API 允许网页在用户提供输入时与 User Agent 交换信息，然后核准或拒绝支付请求。

最重要的是，浏览器起到中介作用，为实现快速结账所需的全部信息均能储存在浏览器中，因此用户只需点击一次便可确认支付。

利用 Payment Request API，可同时为用户和商家打造尽可能无缝的交易流程。

## 添加发货选项 {: #shipping-options}

{% include "web/_shared/helpful.html" %}