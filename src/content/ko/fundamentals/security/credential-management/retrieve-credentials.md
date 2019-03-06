project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml

{# wf_updated_on: 2016-11-08 #} {# wf_published_on: 2016-11-08 #}

# 인증 정보 검색 {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/megginkearney.html" %}

사용자를 로그인시키려면 브라우저의 비밀번호 관리자에서 검색한 인증 정보를 사용하여 사용자 로그인을 수행합니다.

## 인증 정보 가져오기

사용자의 인증 정보를 검색하려면 인증 정보 객체를 인수로 확인하는 프라미스를 반환하는 `navigator.credentials.get()`을 사용하세요. 가져온 인증 정보 객체는 [`PasswordCredential`](#authenticate_with_a_server) 또는 [`FederatedCredential`](#authenticate_with_an_identity_provider)일 수 있습니다. 인증 정보가 없으면 `null`이 반환됩니다.

사용자를 자동으로 로그인시키려면 사용자가 웹사이트를 방문하자마자 `unmediated: true`로 인증 정보 객체를 요청하세요. 예를 들면 다음과 같습니다.

1. Get credential information.
2. Authenticate the user.
3. Update the UI or proceed to the personalized page.

### `navigator.credentials.get` 매개변수 {: .hide-from-toc }

이 요청은 인증 정보 객체로 즉시 확인되고 계정 선택기를 표시하지 않습니다. 브라우저가 사용자 인증 정보를 획득하면 알림 메시지가 뜹니다.

사용자에게 중재가 필요하거나 여러 계정이 있을 경우 사용자가 일반적인 로그인 양식을 건너뛰고 로그인할 수 있도록 하려면 계정 선택기를 사용하세요.

* 사용자가 자동 로그인 기능을 확인하지 않았을 때(브라우저 인스턴스당 1회)
* 사용자에게 인증 정보가 없거나 출처에 2개보다 많은 인증 정보 객체가 있을 때
* 사용자가 출처에 대한 사용자 중재가 필요하다는 요청을 했을 때

계정 선택기는 보통 사용자가 'Sign-In' 버튼을 탭할 때 호출됩니다. 사용자는 예컨대 다음과 같이 로그인할 계정을 선택할 수 있습니다.

    navigator.credentials.get({
      password: true,
      unmediated: false,
      federated: {
        providers: [
          'https://account.google.com',
          'https://www.facebook.com'
        ]
      }
    }).then(function(cred) {
      if (cred) {
        // Use provided credential to sign user in  
      }
    });
    

계정 선택기를 활성화하려면 `unmediated` 속성을 `false`로 설정하세요.

사용자가 자신이 사용하고 싶은 계정을 선택하고 나면 프라미스가 사용자의 선택을 바탕으로 `PasswordCredential` 또는 `FederatedCredential`로 확인합니다. 그런 다음, [인증 정보 유형을 결정](#determine-credential-type)하고 제공된 인증 정보로 사용자를 인증합니다.

### 자동으로 인증 정보 가져오기

사용자가 계정 선택기를 취소하거나 저장된 인증 정보가 없으면 프라미스가 `undefined` 값으로 확인합니다. 그럴 경우에는 로그인 양식 환경으로 돌아갑니다.

    }).then(c => {
     if (c) {
       switch (c.type) {
         case 'password':
           return sendRequest(c);
           break;
         case 'federated':
           return gSignIn(c);
           break;
       }
     } else {
       return Promise.resolve();
     }
    

`navigator.credentials.get()`이 확인되면 `undefined` 또는 Credential 객체를 반환합니다. `PasswordCredential` 또는 `FederatedCredential`인지 판단하려면 `password` 또는 `federated` 중 하나인 객체의 `.type` 속성만 살펴보면 됩니다.

### 계정 선택기를 통해 인증 정보 가져오기

`.type`이 `federated`일 경우 `.provider` 속성은 ID 제공자를 나타내는 문자열입니다.

    }).then(profile => {
     if (profile) {
       updateUI(profile);
     }
    

### 사용자 이름과 비밀번호로 인증

예:

<div>
  <figure>
    <img src="imgs/auto-sign-in.png" alt="Blue toast showing user is signing in.">
  </figure>
</div>

`undefined` 값의 경우, 사용자가 로그아웃된 상태로 계속 진행합니다.

        if (cred) {
      switch (cred.type) {
        case 'password':
          // authenticate with a server
          break;
        case 'federated':
          switch (cred.provider) {
            case 'https://accounts.google.com':
              // run google identity authentication flow
              break;
            case 'https://www.facebook.com':
              // run facebook identity authentication flow
              break;
          }
          break;
      }
    } else {
      // auto sign-in not possible
    }
    

### ID 제공자로 인증

    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="id"
    
    chromedemojp@gmail.com
    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="password"
    
    testtest
    ------WebKitFormBoundaryOkstjzGAv8zab97W--
    

## 인증 정보 유형 결정 {: #determine-credential-type }

다음과 같은 경우에는 `undefined` 값이 전달됩니다.

<div>
  <figure>
    <img src="imgs/account-chooser.png" alt="Google account chooser showing multiple accounts.">
  </figure>
</div>

서버를 이용해 사용자를 인증하려면 `fetch()`를 사용하여 제공되는 `PasswordCredential`을 서버에 POST하세요.

1. Get credential information and show account chooser.
2. [Authenticate the user](#authenticate_user).
3. [Update UI or proceed to a personalized page](#update_ui).

### Get credential information and show account chooser

POST된 `fetch`는 다음과 같이 `PasswordCredential` 객체를 `multipart/form-data`로 인코딩된 `FormData` 객체로 자동 변환합니다.

참고: `XMLHttpRequest`를 사용하여 `PasswordCredential`을 서버에 POST할 수 없습니다.

가져온 `PasswordCredential` 객체에는 다음 매개변수가 포함됩니다.

    if (cred) {
      if (cred.type == 'password') {
        // Use `email` instead of `id` for the id
        cred.idName = 'email';
    
        // Append CSRF Token
        var csrf_token = document.querySelector('#csrf_token').value;
        var form = new FormData();
        form.append('csrf_token', csrf_token);
    
        // Append additional credential data to `.additionalData`
        cred.additionalData = form;
    
        // `POST` the credential object.
        // id, password and the additional data will be encoded and
        // sent to the url as the HTTP body.
        fetch(url, {           // Make sure the URL is HTTPS
          method: 'POST',      // Use POST
          credentials: cred    // Add the password credential object
        }).then(function() {
          // continuation
        });
      }
    }
    

어떤 경우에는 인증 POST에 데이터를 추가로 더해야 할 때도 있습니다.

### Don't forget to fallback to sign-in form

문자열을 `.idName` 또는 `.passwordName`에 할당하여 param 키를 변경하세요.

* No credentials are stored.
* The user dismissed the account chooser without selecting an account.
* The API is not available.

<div class="clearfix"></div>

    // Instantiate an auth object
    var auth2 = gapi.auth2.getAuthInstance();
    
    // Is this user already signed in?
    if (auth2.isSignedIn.get()) {
      var googleUser = auth2.currentUser.get();
    
      // Same user as in the credential object?
      if (googleUser.getBasicProfile().getEmail() === id) {
        // Continue with the signed-in user.
        return Promise.resolve(googleUser);
      }
    }
    
    // Otherwise, run a new authentication flow.
    return auth2.signIn({
      login_hint: id || ''
    });
    

### Full code example

    // After a user signing out...
    navigator.credentials.requireUserMediation();
    

## 사용자 인증

또한, `.additionalData`를 `FormData`에 할당하여 사이트 간 요청 위조(CSRF) 토큰과 같은 여분의 매개변수를 추가하고 그 매개변수에 키-값을 추가할 수도 있습니다.

사용자 인증 정보 객체를 가져온 후 다음을 수행합니다.

1. Authenticate the user with a third-party identity.
2. Store the identity information.
3. [Update UI or proceed to a personalized page](#update_ui) (same as auto sign-in).

### Authenticate user with third-party identity

`FormData` 대신 `URLSearchParams` 객체를 `.additionalData`에 할당해도 비슷한 결과를 얻을 수 있습니다. 이 경우에는 전체 인증 정보 객체가 `application/x-www-form-urlencoded`를 사용하여 인코딩됩니다.

ID 제공자로 사용자를 인증하려면 그냥 `FederatedCredential`을 사용하는 특정한 인증 흐름을 사용하세요.

    navigator.credentials.get({
      password: true,
      mediation: 'optional',
      federated: {
        providers: [
          'https://account.google.com'
        ]
      }
    }).then(function(cred) {
      if (cred) {
    
        // Instantiate an auth object
        var auth2 = gapi.auth2.getAuthInstance();
    
        // Is this user already signed in?
        if (auth2.isSignedIn.get()) {
          var googleUser = auth2.currentUser.get();
    
          // Same user as in the credential object?
          if (googleUser.getBasicProfile().getEmail() === cred.id) {
            // Continue with the signed-in user.
            return Promise.resolve(googleUser);
          }
        }
    
        // Otherwise, run a new authentication flow.
        return auth2.signIn({
          login_hint: id || ''
        });
    
      }
    });
    

예를 들어, 제공자가 Google이라면 다음과 같이 [Google Sign-In 자바스크립트 라이브러리](/identity/sign-in/web/)를 사용하세요.

Google Sign-In은 세션을 생성하기 위해 서버로 보내는 인증 증명으로 ID 토큰을 생성합니다.

* [Google Sign-In](/identity/sign-in/web/)
* [Facebook Login](https://developers.facebook.com/docs/facebook-login)
* [Twitter Sign-in](https://dev.twitter.com/web/sign-in/implementing)
* [GitHub OAuth](https://developer.github.com/v3/oauth/)

### Store identity information

추가적인 ID 제공자에 대해서는 다음 각각의 문서를 참조하세요.

사용자가 웹사이트에서 로그아웃하는 경우 사용자가 다음에 방문할 때 자동으로 로그인되지 않도록 하는 것은 개발자의 책임입니다. 자동 로그인을 해제하려면 [`navigator.credentials.requireUserMediation()`](https://developer.mozilla.org/en-US/docs/Web/API/CredentialsContainer/requireUserMediation)을 호출하세요.

그런 다음, `navigator.credentials.get()`이 `unmediated: true`와 함께 호출되면 `undefined`를 반환하고 사용자가 로그인되지 않게 됩니다. 이 설정은 이 출처에 대한 현재 브라우저 인스턴스에 대해서만 기억됩니다.

사용자가 계정 선택기에서 로그인하고 싶은 계정을 선택하여 의도적으로 로그인하는 동작을 선택하면 자동 로그인을 계속 사용할 수 있습니다. 그러면 사용자가 명시적으로 로그아웃할 때까지는 사이트를 방문할 때마다 항상 로그인된 상태로 유지됩니다.

    // Create credential object synchronously.
    var cred = new FederatedCredential({
      id:       id,                           // id in IdP
      provider: 'https://account.google.com', // A string representing IdP
      name:     name,                         // name in IdP
      iconURL:  iconUrl                       // Profile image url
    });
    

{# wf_devsite_translation #}

    // Create credential object asynchronously.
    var cred = await navigator.credentials.create({
      federated: {
        id:       id,
        provider: 'https://accounts.google.com',
        name:     name,
        iconURL:  iconUrl
      }
    });
    

Then store the credential object:

    // Store it
    navigator.credentials.store(cred)
    .then(function() {
      // continuation
    });
    

## 로그아웃 {: #sign-out }

Sign out your users when the sign-out button is tapped. First terminate the session, then turn off auto sign-in for future visits. (How you terminate your sessions is totally up to you.)

### Turn off auto sign-in for future visits

Call `navigator.credentials.preventSilentAccess()`:

    signoutUser();
    if (navigator.credentials && navigator.credentials.preventSilentAccess) {
      navigator.credentials.preventSilentAccess();
    }
    

This will ensure the auto sign-in won’t happen until next time the user enables auto sign-in. To resume auto sign-in, a user can choose to intentionally sign-in by choosing the account they wish to sign in with, from the account chooser. Then the user is always signed back in until they explicitly sign out.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}