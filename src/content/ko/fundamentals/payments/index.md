project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Payment Request API는 웹 환경에서 빠르고 쉬운 결제를 위한 것입니다.

{# wf_published_on: 2016-07-25 #} {# wf_updated_on: 2016-12-06 #}

# Payment Request API: 통합 가이드 {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/zkoch.html" %}

<div class="video-wrapper-full-width">
  <iframe class="devsite-embedded-youtube-video" data-video-id="colCcgKoLUM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

## Payment Request API 소개 {: #introducing }

Dogfood: `PaymentRequest`는 여전히 개발 중에 있습니다. 이 API가 구현하기에 충분히 안정적이라고 생각하지만, 계속 변경될 가능성이 있습니다. 이 API의 현재 상태를 항상 반영하도록 이 페이지를 지속적으로 업데이트할 것입니다([M56 변경 사항](https://docs.google.com/document/d/1I8ha1ySrPWhx80EB4CVPmThkD4ILFM017AfOA5gEFg4/edit#)). 한편, 이전 버전과 호환되지 않을 수도 있는 API 변경 사항으로부터 스스로를 보호할 수 있도록 사이트에 삽입할 수 있는 [심](https://storage.googleapis.com/prshim/v1/payment-shim.js)을 제공하고 있습니다. 심은 두 가지 주요 Chrome 버전에 대한 모든 API 차이점을 숨겨줍니다.

상품을 온라인으로 구입하는 것은 편리하지만, 종종 짜증스러운 경험이기도 합니다. 특히, 휴대기기인 경우 그렇습니다. 모바일 트래픽이 계속해서 증가하기는 하지만 모바일 전환은 전체 완료된 구매의 1/3 정도만 처리합니다. 다시 말해, 사용자는 데스크톱 구매 시보다 모바일 구매 시 구매 포기율이 2배 더 높습니다. 그 이유는 무엇일까요?

**For consumers**, they simplify checkout flow, by making it a few taps instead of typing small characters many times on a virtual keyboard.

**For merchants**, they make it easier to implement with a variety of payment options already filtered for the customer.

온라인 구매 양식은 사용자 작업이 많고, 사용이 어려우며, 로드 및 새로고침이 느리고, 여러 단계를 완료해야 합니다. 그 이유는 두 가지 주요 온라인 결제 구성 요소인 보안 및 편의성이 종종 서로 어긋나서 작동하기 때문입니다. 즉, 하나가 더 좋으면 다른 하나는 상대적으로 나쁘기 때문입니다.

포기를 초래하는 대부분의 문제는 구매 양식과 직접적으로 관련이 있을 수 있습니다. 각각의 앱이나 사이트는 고유한 데이터 입력 및 검증 프로세스를 가지며, 사용자는 흔히 각 앱의 구매 지점에서 동일한 정보를 입력해야 한다는 것을 알게 됩니다. 또한, 애플리케이션 개발자는 여러 고유한 결제 방법을 지원하는 구매 흐름을 생성하는 데 어려움을 겪고 있습니다. 결제 방법 요구사항에 사소한 차이가 있다 하더라도 이러한 차이는 양식 작성 및 제출 프로세스를 복잡하게 만들 수 있습니다.

## Payment Request API 사용 {: #using }

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

## 배송 주소 수집 {: #shipping-address }

이러한 문제의 하나 이상을 개선하거나 해결하는 시스템이 있다면 무엇이든 정말 반가운 변화입니다. [자동완성](/web/updates/2015/06/checkout-faster-with-autofill) 기능으로 이미 문제 해결을 시작하기는 했지만, 이제 더욱 종합적인 해결책에 대해 알아보도록 하겠습니다.

Payment Request API는 *체크아웃 양식을 없애는 데* 도움이 되는 시스템입니다. 이 API는 더욱 일관된 사용자 환경을 제공하고 웹 판매자가 이질적인 결제 방법을 손쉽게 활용할 수 있도록 지원함으로써 구매 프로세스 도중 사용자 워크플로를 상당히 향상시킵니다. Payment Request API는 새로운 결제 방법이 아니며, 결제 프로세서와 직접 통합되지도 않으며, 목표가 다음과 같은 프로세스 계층입니다.

Payment Request API는 판매자가 단일 API 호출에서 결제를 요청하고 승인할 수 있도록 허용함으로써 기존의 체크아웃 흐름을 대체하는 다중 브라우저(cross-browser) 지원 오픈 표준입니다. Payment Request API를 사용하면 사용자가 결제 요청을 승인하거나 거부하기 전에 입력을 제공하는 동시에 웹페이지에서 사용자 에이전트와 정보를 교환할 수 있습니다.

무엇보다도, 중개자 역할을 하는 브라우저에서는 빠른 체크아웃에 필요한 모든 정보를 브라우저에 저장할 수 있으므로 사용자는 한 번의 클릭만으로 확인 및 결제 작업을 수행할 수 있습니다.

Payment Request API를 사용하면 트랜잭션 프로세스가 사용자와 판매자 모두에게 최대한 원활하게 수행됩니다.

## 배송 옵션 추가 {: #shipping-options}

{% include "web/_shared/helpful.html" %}