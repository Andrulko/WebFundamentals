project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-11-08 #}

# Ambil Kredensial {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/megginkearney.html" %}

Untuk memasukkan pengguna, ambil kredensial dari pengelola sandi di browser dan gunakan untuk memproses masuknya pengguna.

## Dapatkan kredensial

Untuk mengambil kredensial pengguna, gunakan `navigator.credentials.get()`, yang mengembalikan sebuah promise yang akan memproses dengan objek kredensial sebagai argumen. Objek kredensial yang diperoleh bisa berupa [`PasswordCredential`](#authenticate_with_a_server) atau [`FederatedCredential`](#authenticate_with_an_identity_provider). Jika tidak ada informasi kredensial, `null` akan dikembalikan.

Untuk memasukkan pengguna secara otomatis, minta objek kredensial dengan `unmediated: true`, segera setelah pengguna mendarat di situs web Anda, misalnya:

1. Get credential information.
2. Authenticate the user.
3. Update the UI or proceed to the personalized page.

### Parameter `navigator.credentials.get` {: .hide-from-toc }

Permintaan ini segera diproses dengan objek kredensial dan tidak akan menampilkan pemilih akun. Bila browser memperoleh informasi kredensial, notifikasi akan muncul:

Jika pengguna memerlukan mediasi, atau memiliki beberapa akun, gunakan pemilih akun agar pengguna bisa masuk, dan melewati formulir masuk biasa.

* Pengguna belum mengakui fitur proses masuk otomatis (sekali per instance browser).
* Pengguna tidak memiliki kredensial atau lebih dari dua objek kredensial telah disimpan di asalnya.
* Pengguna telah meminta untuk mengharuskan mediasi pengguna ke asalnya.

Pemilih akun biasanya dipanggil bila pengguna mengetuk tombol "Sign-In". Pengguna bisa memilih akun untuk masuk, misalnya:

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
    

Untuk mengaktifkan pemilih akun, setel properti `unmediated` ke `false`:

Setelah pengguna memilih akun yang ingin digunakan, promise akan memproses baik dengan `PasswordCredential` atau `FederatedCredential` berdasarkan pilihan mereka. Kemudian, [tentukan tipe kredensial](#determine-credential-type) dan autentikasi pengguna dengan kredensial yang disediakan.

### Dapatkan kredensial secara otomatis

Jika pengguna membatalkan pemilih akun atau tidak ada kredensial yang tersimpan, promise memproses dengan nilai `undefined`. Dalam hal ini, mundur ke pengalaman formulir masuk.

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
    

Bila `navigator.credentials.get()` diproses, maka akan mengembalikan `undefined` atau objek Credential. Untuk menentukan apakah ini `PasswordCredential` atau `FederatedCredential`, cukup lihat ke properti `.type` objek tersebut, yang akan berupa `password` atau `federated`.

### Dapatkan kredensial melalui pemilih akun

Jika `.type` adalah `federated`, properti `.provider` adalah string yang menyatakan penyedia identitas.

    }).then(profile => {
     if (profile) {
       updateUI(profile);
     }
    

### Autentikasi dengan nama pengguna dan sandi

Misalnya:

<div>
  <figure>
    <img src="imgs/auto-sign-in.png" alt="Blue toast showing user is signing in.">
  </figure>
</div>

Dalam hal nilai `undefined`, lanjutkan dengan pengguna dalam keadaan telah dikeluarkan.

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
    

### Autentikasi dengan penyedia identitas

    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="id"
    
    chromedemojp@gmail.com
    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="password"
    
    testtest
    ------WebKitFormBoundaryOkstjzGAv8zab97W--
    

## Tentukan tipe kredensial {: #determine-credential-type }

Nilai `undefined` akan diteruskan bila:

<div>
  <figure>
    <img src="imgs/account-chooser.png" alt="Google account chooser showing multiple accounts.">
  </figure>
</div>

Untuk mengautentikasi pengguna dengan server Anda, POST `PasswordCredential` yang disediakan ke server dengan menggunakan `fetch()`.

1. Get credential information and show account chooser.
2. [Authenticate the user](#authenticate_user).
3. [Update UI or proceed to a personalized page](#update_ui).

### Get credential information and show account chooser

Bila telah di-POST-kan, `fetch` secara otomatis mengonversi objek `PasswordCredential` ke objek `FormData` yang dienkodekan sebagai `multipart/form-data`:

Note: Anda tidak bisa menggunakan `XMLHttpRequest` untuk mem-POST-kan `PasswordCredential` ke server Anda.

Objek `PasswordCredential` yang diperoleh menyertakan parameter berikut:

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
    

Dalam beberapa kasus, mungkin perlu menambahkan data tambahan ke POST autentikasi.

### Don't forget to fallback to sign-in form

Ubah kunci param dengan menetapkan string ke `.idName` atau `.passwordName`.

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
    

## Autentikasi pengguna

Anda juga bisa menambahkan parameter ekstra seperti token cross-site request forgery (CSRF) dengan menetapkan `.additionalData` ke `FormData` dan menambahkan nilai-kunci padanya.

Setelah Anda mendapatkan objek kredensial:

1. Authenticate the user with a third-party identity.
2. Store the identity information.
3. [Update UI or proceed to a personalized page](#update_ui) (same as auto sign-in).

### Authenticate user with third-party identity

Anda bisa melakukan hal serupa dengan menetapkan objek `URLSearchParams` sebagai ganti `FormData` ke `.additionalData`. Dalam hal ini, keseluruhan objek kredensial dienkodekan menggunakan `application/x-www-form-urlencoded`.

Untuk mengautentikasi pengguna dengan penyedia identitas, cukup gunakan aliran autentikasi spesifik bersama `FederatedCredential`.

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
    

Misalnya, jika penyedianya adalah Google, gunakan [pustaka JavaScript Google Sign-In](/identity/sign-in/web/):

Google Sign-In menghasilkan token ID sebagai bukti autentikasi yang Anda kirimkan ke server untuk membuat sesi.

* [Google Sign-In](/identity/sign-in/web/)
* [Facebook Login](https://developers.facebook.com/docs/facebook-login)
* [Twitter Sign-in](https://dev.twitter.com/web/sign-in/implementing)
* [GitHub OAuth](https://developer.github.com/v3/oauth/)

### Store identity information

Untuk penyedia identitas tambahan, lihat dokumentasinya masing-masing:

Bila pengguna keluar dari situs web, Anda bertanggung jawab untuk memastikan pengguna tidak secara otomatis dimasukkan pada kunjungan berikutnya. Untuk menonaktifkan masuk otomatis, panggil [`navigator.credentials.requireUserMediation()`](https://developer.mozilla.org/en-US/docs/Web/API/CredentialsContainer/requireUserMediation):

Kemudian, jika `navigator.credentials.get()` dipanggil bersama `unmediated: true`, maka akan mengembalikan `undefined` dan pengguna tidak akan dimasukkan. Ini hanya akan diingat untuk instance browser saat ini bagi asal ini.

Untuk melanjutkan masuk otomatis, pengguna bisa sengaja memilih masuk, dengan memilih akun yang ingin digunakan untuk masuk, dari pemilih akun. Maka, pengguna akan selalu dimasukkan kembali, hingga mereka secara eksplisit keluar.

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
    

## Keluar {: #sign-out }

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