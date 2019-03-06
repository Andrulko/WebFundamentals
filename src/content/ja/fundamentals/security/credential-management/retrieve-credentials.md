project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml

{# wf_updated_on:2016-11-08 #} {# wf_published_on:2016-11-08 #}

# 認証情報を取得する {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/megginkearney.html" %}

{% include "web/_shared/translation-out-of-date.html" %}

## 認証情報を取得する

ユーザーのログインを実行するには、ブラウザのパスワード マネージャーから認証情報を取得し、その情報を使用してログイン処理を行う必要があります。

ユーザーの認証情報を取得するには、認証オブジェクトを引数に指定して解決される Promise を返す `navigator.credentials.get()` を使用します。取得される認証オブジェクトは、[`PasswordCredential`](#authenticate_with_a_server) または [`FederatedCredential`](#authenticate_with_an_identity_provider) のいずれかです。 認証情報が存在しない場合は、`null` が返されます。

1. Get credential information.
2. Authenticate the user.
3. Update the UI or proceed to the personalized page.

### `navigator.credentials.get` パラメータ{: .hide-from-toc }

ユーザーのログインを自動的に実行するには、ユーザーがウェブサイトにアクセスした直後に、次のように `unmediated: true` で認証オブジェクトをリクエストします。

このリクエストは認証オブジェクトで即座に解決され、Account Chooser は表示されません。 ブラウザが認証情報を取得すると、通知がポップアップ表示されます。

* ユーザーが自動ログイン機能を認識しなかった（ブラウザ インスタンスごとに一度）。
* ユーザーの認証情報がオリジンに保存されていない、または 3 つ以上の認証オブジェクトが保存されている。
* ユーザーがオリジンにユーザー メディエーションが必要であることをリクエストしている。

ユーザーがメディエーションを要求するか、複数のアカウントを持っている場合は、Account Chooser を使用してユーザーのログインを実行し、通常のログイン フォームをスキップします。

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
    

通常、Account Chooser は、ユーザーが [Sign-In] ボタンをタップしたときに表示されます。 図のように、ユーザーはアカウントを選択してログインできます。

Account Chooser を有効にするには、`unmediated` プロパティを `false` に設定します。

### 認証情報を自動的に取得する

ユーザーが使用するアカウントを選択したら、その選択に基づいて、`PasswordCredential` または `FederatedCredential` で Promise が解決されます。 その後、[認証情報のタイプを決定し](#determine-credential-type)、提供された認証情報でユーザーを認証します。

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
    

ユーザーが Account Chooser をキャンセルするか、保存されている認証情報がない場合は、`undefined` 値で Promise が解決されます。 この場合、ログイン フォームにフォールバックします。

### Account Chooser で認証情報を取得する

`navigator.credentials.get()` が解決されると、`undefined` または認証オブジェクトが返されます。 `PasswordCredential` と `FederatedCredential` のどちらで解決するかを決定するには、オブジェクトの `.type` プロパティを確認します。このプロパティは、`password` または `federated` です。

    }).then(profile => {
     if (profile) {
       updateUI(profile);
     }
    

### ユーザー名とパスワードで認証する

`.type` が `federated` である場合、`.provider` プロパティは、ID プロバイダを表す文字列です。

<div>
  <figure>
    <img src="imgs/auto-sign-in.png" alt="Blue toast showing user is signing in.">
  </figure>
</div>

次に例を示します。

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
    

### ID プロバイダによって認証する

    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="id"
    
    chromedemojp@gmail.com
    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="password"
    
    testtest
    ------WebKitFormBoundaryOkstjzGAv8zab97W--
    

## 認証情報のタイプを決定する{: #determine-credential-type }

`undefined` 値の場合は、ユーザーをログアウトした状態として扱います。

<div>
  <figure>
    <img src="imgs/account-chooser.png" alt="Google account chooser showing multiple accounts.">
  </figure>
</div>

`undefined` 値は、次の場合に返されます。

1. Get credential information and show account chooser.
2. [Authenticate the user](#authenticate_user).
3. [Update UI or proceed to a personalized page](#update_ui).

### Get credential information and show account chooser

サーバーでユーザーを認証するには、`fetch()` を使用して、提供された `PasswordCredential` をサーバーに POST 送信します。

POST 送信すると、`fetch` によって、`PasswordCredential` オブジェクトが、`multipart/form-data` としてエンコードされた `FormData` オブジェクトに自動的に変換されます。

注: `XMLHttpRequest` を使用して、`PasswordCredential` をサーバーに POST 送信することはできません。

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
    

取得した `PasswordCredential` オブジェクトには、次のパラメータが含まれます。

### Don't forget to fallback to sign-in form

POST 認証にデータを追加する必要がある場合があります。

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
    

## ユーザーを認証する

パラメータキーを変更するには、文字列を `.idName` または `.passwordName` に指定します。

`.additionalData` を `FormData` に指定して、それにキー値を付加することにより、クロスサイト リクエスト フォージェリ（CSRF）トークンなどのパラメータを追加することもできます。

1. Authenticate the user with a third-party identity.
2. Store the identity information.
3. [Update UI or proceed to a personalized page](#update_ui) (same as auto sign-in).

### Authenticate user with third-party identity

認証オブジェクトを取得したら、次のコードを実行できます。

`FormData` の代わりに、`URLSearchParams` オブジェクトを `.additionalData` に指定して、同じような処理を実行できます。 この場合、`application/x-www-form-urlencoded` を使用して、認証オブジェクト全体がエンコードされます。

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
    

ID プロバイダによってユーザーを認証するには、`FederatedCredential` を使用した特定の認証フローを実行します。

たとえば、プロバイダが Google の場合は、[Google Sign-In JavaScript ライブラリ](/identity/sign-in/web/)を使用します。

* [Google Sign-In](/identity/sign-in/web/)
* [Facebook Login](https://developers.facebook.com/docs/facebook-login)
* [Twitter Sign-in](https://dev.twitter.com/web/sign-in/implementing)
* [GitHub OAuth](https://developer.github.com/v3/oauth/)

### Store identity information

Google Sign-In では、ID トークンが認証の証明になり、この ID をサーバーに送信してセッションを作成します。

その他の ID プロバイダについては、対応するドキュメントをご覧ください。

ユーザーがウェブサイトからログアウトした場合、ユーザーが次にウェブサイトにアクセスしたときに、自動的にログインしないようにする必要があります。 自動ログオンをオフにするには、[`navigator.credentials.requireUserMediation()`](https://developer.mozilla.org/en-US/docs/Web/API/CredentialsContainer/requireUserMediation) を呼び出します。

その後、`unmediated: true` を指定して `navigator.credentials.get()` を呼び出すと、`undefined` が返され、ユーザーはログインしないようになります。 自動ログインのオフは、このオリジンの現在のブラウザ インスタンスに対してのみ記憶されます。

    // Create credential object synchronously.
    var cred = new FederatedCredential({
      id:       id,                           // id in IdP
      provider: 'https://account.google.com', // A string representing IdP
      name:     name,                         // name in IdP
      iconURL:  iconUrl                       // Profile image url
    });
    

再び自動ログインを有効にするには、ユーザー側で Account Chooser からログインするアカウントを選択して、意図的にログインする必要があります。 その後、ユーザーは明示的にログアウトするまで、常に再ログインするようになります。

    // Create credential object asynchronously.
    var cred = await navigator.credentials.create({
      federated: {
        id:       id,
        provider: 'https://accounts.google.com',
        name:     name,
        iconURL:  iconUrl
      }
    });
    

{# wf_devsite_translation #}

    // Store it
    navigator.credentials.store(cred)
    .then(function() {
      // continuation
    });
    

## ログアウト {: #sign-out }

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