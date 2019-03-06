project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml

{# wf_updated_on:2016-11-08 #} {# wf_published_on:2016-11-08 #}

# 检索凭据 {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/megginkearney.html" %}

要使用户登录，请从浏览器的密码管理器检索凭据，并使用这些凭据让用户登录。

## 获取凭据

要检索用户的凭据，请使用 `navigator.credentials.get()`，其返回一个使用凭据对象作为参数进行解析的 promise。获取的凭据对象可以是 [`PasswordCredential`](#authenticate_with_a_server) 或 [`FederatedCredential`](#authenticate_with_an_identity_provider)。如果凭据信息不存在，则会返回 `null`。

要让用户自动登录，请在用户访问您的网站时使用 `unmediated: true` 请求一个凭据对象，例如：

1. Get credential information.
2. Authenticate the user.
3. Update the UI or proceed to the personalized page.

### `navigator.credentials.get` 参数 {: .hide-from-toc }

此请求将立即使用一个凭据对象进行解析，并且不会显示帐户选择器。 当浏览器获取凭据信息时，系统将弹出一个通知：

如果用户需要调节，或具有多个帐户，则使用帐户选择器让用户登录，从而跳过普通的登录表单。

* 用户尚未确认自动登录功能（每个浏览器实例确认一次）。
* 用户没有凭据，或在源中存储了两个以上的凭据对象。
* 用户已请求用户对源进行调节。

当用户点按“Sign-In”按钮时，通常会调用帐户选择器。 用户可以选择一个帐户进行登录，例如：

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
    

要启用帐户选择器，请将 `unmediated` 属性设置为 `false`：

在用户选择了他们要使用的帐户后，promise 将基于他们的选择使用 `PasswordCredential` 或 `FederatedCredential` 进行解析。然后，[确定凭据类型](#determine-credential-type)并使用提供的凭据对用户进行身份验证。

### 自动获取凭据

如果用户取消帐户选择器或没有存储凭据，则 promise 使用一个 `undefined` 值进行解析。 在此情况下，回退到登录表单体验。

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
    

当 `navigator.credentials.get()` 进行解析时，它将返回 `undefined` 或 Credential 对象。 要确定它是 `PasswordCredential` 还是 `FederatedCredential`，只需查看此对象的 `.type` 属性，即 `password` 或 `federated`。

### 通过帐户选择器获取凭据

如果 `.type` 是 `federated`，则 `.provider` 属性是一个表示身份提供程序的字符串。

    }).then(profile => {
     if (profile) {
       updateUI(profile);
     }
    

### 使用用户名和密码进行身份验证

例如：

<div>
  <figure>
    <img src="imgs/auto-sign-in.png" alt="Blue toast showing user is signing in.">
  </figure>
</div>

如果是一个 `undefined` 值，则用户继续处于退出状态。

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
    

### 使用身份提供程序进行身份验证

    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="id"
    
    chromedemojp@gmail.com
    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="password"
    
    testtest
    ------WebKitFormBoundaryOkstjzGAv8zab97W--
    

## 确定凭据类型{: #determine-credential-type }

当出现以下情况时传递一个 `undefined` 值：

<div>
  <figure>
    <img src="imgs/account-chooser.png" alt="Google account chooser showing multiple accounts.">
  </figure>
</div>

要向服务器验证用户的身份，请使用 `fetch()` 将提供的 `PasswordCredential` POST 到服务器。

1. Get credential information and show account chooser.
2. [Authenticate the user](#authenticate_user).
3. [Update UI or proceed to a personalized page](#update_ui).

### Get credential information and show account chooser

完成 POST 后，`fetch` 自动将 `PasswordCredential` 对象转换为使用 `multipart/form-data` 编码的 `FormData` 对象：

Note: 您不能使用 `XMLHttpRequest` 将 `PasswordCredential` POST 到您的服务器。

获取的 `PasswordCredential` 对象包括以下参数：

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
    

在某些情况下，可能必须将附加数据添加到身份验证 POST。

### Don't forget to fallback to sign-in form

通过向 `.idName` 或 `.passwordName` 分配一个字符串来更改参数键。

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
    

## 对用户进行身份验证

您也可以通过向 `FormData` 分配一个 `.additionalData` 来添加额外参数（如跨站点请求伪造 (CSRF) 令牌），并向该参数追加键值。

获取凭据对象后：

1. Authenticate the user with a third-party identity.
2. Store the identity information.
3. [Update UI or proceed to a personalized page](#update_ui) (same as auto sign-in).

### Authenticate user with third-party identity

您可以通过向 `.additionalData` 分配一个 `URLSearchParams` 对象（而非 `FormData`）来执行相似操作。 在此情况下，使用 `application/x-www-form-urlencoded` 对整个凭据对象进行编码。

要通过身份提供程序对用户进行身份验证，使用具有 `FederatedCredential` 的特定身份验证流程即可。

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
    

例如，如果提供程序为 Google，则使用 [Google Sign-In JavaScript 内容库](/identity/sign-in/web/)：

Google Sign-In 会生成一个 id 令牌作为身份验证的证明，您将该令牌发送到服务器以创建一个会话。

* [Google Sign-In](/identity/sign-in/web/)
* [Facebook Login](https://developers.facebook.com/docs/facebook-login)
* [Twitter Sign-in](https://dev.twitter.com/web/sign-in/implementing)
* [GitHub OAuth](https://developer.github.com/v3/oauth/)

### Store identity information

有关其他身份提供程序，请参阅相应的文档：

当用户退出您的网站时，您需要确保此用户在下次访问时不会自动登录。 要关闭自动登录，请调用 [`navigator.credentials.requireUserMediation()`](https://developer.mozilla.org/en-US/docs/Web/API/CredentialsContainer/requireUserMediation)：

然后，如果使用 `unmediated: true` 调用`navigator.credentials.get()`，它将返回 `undefined` 并且用户不会登录。 系统仅针对此源的当前浏览器实例记住这个值。

要继续使用自动登录，用户可以选择主动登录，只需从帐户选择器中选择他们在登录时要使用的帐户。 于是，用户在明确退出前始终可以重新登录。

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
    

## 退出{: #sign-out }

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