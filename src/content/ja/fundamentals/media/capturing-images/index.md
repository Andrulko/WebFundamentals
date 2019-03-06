project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: ほとんどのブラウザはユーザーのカメラにアクセスできます。

{# wf_updated_on:2016-08-22 #} {# wf_published_on:2016-08-23 #}

# ユーザーから画像を取得する {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

現在、多くのブラウザには、ユーザーによる動画および音声ファイルの入力を処理する機能が備わっています。 ただしブラウザによっては、この機能が動的に組み込まれている場合や、ユーザーの端末上にある別のアプリに処理が委ねられる場合があります。

## 簡単なケースから始める

最も簡単な方法は、事前に撮影済みのファイルをユーザーに要求することです。 そのためには、簡単なファイル入力要素を作成して `accept` フィルタを追加し、画像ファイルのみを受け入れる（理想的にはカメラから画像ファイルを直接取得する）ことを示します。

### 単一フレームを取得する

この方法はすべてのプラットフォームで使用できます。PC の場合、ユーザーは、ファイル システムから画像ファイルをアップロードするように求められます。 iOS 上の Safari にこの方法を使用すると、カメラアプリが起動し、画像を取得してウェブページに送信できるようになります。Android の場合、ユーザーは画像をウェブページに送信する前に、画像の取得に使用するアプリを選択できます。

送信されたデータは `<form>` にアタッチできます。または、入力要素の `onchange` イベントをリッスンして、`target` イベントの `files` プロパティを読み取ると、JavaScript でデータを操作することが可能です。

### カメラへのアクセス権を取得する

画像ファイルへのアクセスは簡単です。

    <input type="file" accept="image/*" capture>
    

ファイルにアクセスできるようになると、ファイルに対してあらゆる操作を行えます。たとえば、次の操作が可能です。

![](images/ios-chooser.png) ![](images/android-chooser.png)

最新のブラウザはカメラに直接アクセスできるため、ウェブページと完全に統合されたエクスペリエンスを実現できます。よって、ユーザーはブラウザから離れる必要がありません。

<pre class="prettyprint">&lt;input type="file" accept="image/*" id="file-input">
&lt;script>
  const fileInput = document.getElementById('file-input');

  fileInput.addEventListener('change', (e) => doSomethingWithFiles(e.target.files));
&lt;/script>
</pre>

警告:カメラに直接アクセスする機能は強力なので、ユーザーの同意を得るとともに、サイトを安全なオリジン（HTTPS）に配置する必要があります。

WebRTC 仕様の API（`getUserMedia()`）を使用して、カメラやマイクに直接アクセスできます。 この API を使用すると、接続済みのマイクまたはカメラに対するアクセス権の付与を求めるメッセージがユーザーに表示されます。

    <video id="player" controls autoplay></video>
    <script>  
      var player = document.getElementById('player');
    
      var handleSuccess = function(stream) {
        player.srcObject = stream;
      };
    
      navigator.mediaDevices.getUserMedia({video: true})
          .then(handleSuccess);
    </script>
    

アクセスに成功すると、API からカメラデータを含む `MediaStream` が返されます。これを `<video>` 要素にアタッチして再生すると、リアルタイム プレビューを表示できます。または、`<canvas>` にアタッチしてスナップショットを取得することも可能です。

カメラからデータを取得するために、`getUserMedia()` API に渡す constraints オブジェクトで `video: true` を設定しています。

### カメラからスナップショットを取得する

この機能だけでは、動画データを取得して再生することしかできないため、あまり便利ではありません。

カメラの未加工データにアクセスするには、`getUserMedia()` で作成されたストリームを取得して、そのデータを処理する必要があります。 ウェブ上の動画には、`Web Audio` のような専用のストーム処理 API がないため、ユーザーのカメラからスナップショットを取得する際は、やや巧妙な処理が必要になります。

<pre class="prettyprint">&lt;div id="target">You can drag an image file here&lt;/div>
&lt;script>
  const target = document.getElementById('target');

  target.addEventListener('drop', (e) => {
    e.stopPropagation();
    e.preventDefault();

    doSomethingWithFiles(e.dataTransfer.files);
  });

  target.addEventListener('dragover', (e) => {
    e.stopPropagation();
    e.preventDefault();

    e.dataTransfer.dropEffect = 'copy';
  });
&lt;/script>
</pre>

その手順は次のとおりです。

これで完了です。

カメラから取得したデータを canvas に保存すると、そのデータをさまざまな方法で操作できます。 次のような操作が可能です。

### 不要になった時点でカメラからのストリーミングを停止する

カメラが不要になったら、使用を停止することをお勧めします。 カメラを停止すると、端末の電力消費が抑えられ、処理能力が向上します。さらに、ユーザーがアプリを安心して使用できるようになります。

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;button id="capture">Capture&lt;/button>
&lt;canvas id="snapshot" width=320 height=240>&lt;/canvas>
&lt;script>
  var player = document.getElementById('player'); 
  var snapshotCanvas = document.getElementById('snapshot');
  var captureButton = document.getElementById('capture');
  <strong>var videoTracks;</strong>

  var handleSuccess = function(stream) {
    // Attach the video stream to the video element and autoplay.
    player.srcObject = stream;
    <strong>videoTracks = stream.getVideoTracks();</strong>
  };

  captureButton.addEventListener('click', function() {
    var context = snapshot.getContext('2d');
    context.drawImage(player, 0, 0, snapshotCanvas.width, snapshotCanvas.height);

    <strong>// Stop all video streams.
    videoTracks.forEach(function(track) {track.stop()});</strong>
  });

  navigator.mediaDevices.getUserMedia({video: true})
      .then(handleSuccess);
&lt;/script>
</pre>

カメラへのアクセスを停止するには、`getUserMedia()` から返されたストリームの各動画トラックで `stop()` を呼び出します。

ユーザーが、サイトによるカメラへのアクセスを許可したことがない場合、`getUserMedia` を呼び出すと、カメラへのアクセス権をサイトに付与するよう求める画面が表示されます。

ユーザーは、マシン上の高機能なデバイスへのアクセス権を要求されることを好まず、リクエストを拒否する傾向があります。また、プロンプトが表示された理由がわからない場合は、リクエストを無視することもあります。よって、初めてカメラが必要になったときに、一度だけアクセス権を要求するようにしてください。アクセス権が付与されると、ユーザーに再度プロンプトが表示されることはありません。ただし、ユーザーがアクセスを拒否した場合は、ユーザーが手動でカメラのパーミッション設定を変更するまで、カメラにアクセスできなくなります。

### Handling a FileList object

警告:ページの読み込み時にカメラへのアクセス権を求めると、ユーザーが拒否する確率は非常に高くなります。

モバイルおよび PC でのブラウザ実装に関する詳細:

WebRTC 仕様の変更と接頭辞の差異によるアプリへの影響を軽減するために、[adapter.js](https://github.com/webrtc/adapter) shim を使用することをお勧めします。

<pre class="prettyprint">&lt;img id="output">
&lt;script>
  const output = document.getElementById('output');

  function doSomethingWithFiles(fileList) {
    let file = null;

    for (let i = 0; i &lt; fileList.length; i++) {
      if (fileList[i].type.match(/^image\//)) {
        file = fileList[i];
        break;
      }
    }

    if (file !== null) {
      output.src = URL.createObjectURL(file);
    }
  }
&lt;/script>
</pre>

{# wf_devsite_translation #}

Once you have access to the file you can do anything you want with it. For example, you can:

- ファイルを `<canvas>` に直接アタッチして操作可能にする
- ファイルをユーザーの端末にダウンロードする
- ファイルを `XMLHttpRequest` にアタッチして、サーバーにアップロードする

## カメラにインタラクティブにアクセスする

Now that you've covered your bases, it's time to progressively enhance!

Modern browsers can get direct access to cameras, allowing you to build experiences that are fully integrated with the web page, so the user need never leave the browser.

### Acquire access to the camera

You can directly access a camera and microphone by using an API in the WebRTC specification called `getUserMedia()`. This will prompt the user for access to their connected microphones and cameras.

Support for `getUserMedia()` is pretty good, but it isn't yet everywhere. In particular, it is not available in Safari 10 or lower, which at the time of writing is still the latest stable version. However, [Apple have announced](https://webkit.org/blog/7726/announcing-webrtc-and-media-capture/) that it will be available in Safari 11.

It's very simple to detect support, however.

    const supported = 'mediaDevices' in navigator;
    

Warning: Direct access to the camera is a powerful feature. It requires consent from the user, and your site MUST be on a secure origin (HTTPS).

When you call `getUserMedia()`, you need to pass in an object that describes what kind of media you want. These choices are called constraints. There are a several possible constraints, covering things like whether you prefer a front- or rear-facing camera, whether you want audio, and your preferred resolution for the stream.

To get data from the camera, however, you need just one constraint, and that is `video: true`.

If successful the API will return a `MediaStream` that contains data from the camera, and you can then either attach it to a `<video>` element and play it to show a real time preview, or attach it to a `<canvas>` to get a snapshot.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;script>
  const player = document.getElementById('player');

  const constraints = {
    video: true,
  };

  navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
      player.srcObject = stream;
    });
&lt;/script>
</pre>

By itself, this isn't that useful. All you can do is take the video data and play it back. If you want to get an image, you have to do a little extra work.

### Grab a snapshot

Your best supported option for getting an image is to draw a frame from the video to a canvas.

Unlike the Web Audio API, there isn't a dedicated stream processing API for video on the web so you have to resort to a tiny bit of hackery to capture a snapshot from the user's camera.

The process is as follows:

1. カメラから取得したフレームを保持する canvas オブジェクトを作成します。
2. カメラ ストリームにアクセスします。
3. ストリームを video 要素にアタッチします。
4. 正確なフレームを取得する必要がある場合は、`drawImage()` を使用して、video 要素のデータを canvas オブジェクトに追加します。

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;button id="capture">Capture&lt;/button>
&lt;canvas id="canvas" width=320 height=240>&lt;/canvas>
&lt;script>
  const player = document.getElementById('player');
  const canvas = document.getElementById('canvas');
  const context = canvas.getContext('2d');
  const captureButton = document.getElementById('capture');

  const constraints = {
    video: true,
  };

  captureButton.addEventListener('click', () => {
    // Draw the video frame to the canvas.
    context.drawImage(player, 0, 0, canvas.width, canvas.height);
  });

  // Attach the video stream to the video element and autoplay.
  navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
      player.srcObject = stream;
    });
&lt;/script>
</pre>

Once you have data from the camera stored in the canvas you can do many things with it. You could:

- データをサーバーに直接アップロードする
- データをローカルで保存する
- 画像に斬新な効果を適用する

## カメラを適切に使用するためにパーミッションを要求する

### Stop streaming from the camera when not needed

It is good practice to stop using the camera when you no longer need it. Not only will this save battery and processing power, it will also give users confidence in your application.

To stop access to the camera you can simply call `stop()` on each video track for the stream returned by `getUserMedia()`.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;button id="capture">Capture&lt;/button>
&lt;canvas id="canvas" width=320 height=240>&lt;/canvas>
&lt;script>
  const player = document.getElementById('player');
  const canvas = document.getElementById('canvas');
  const context = canvas.getContext('2d');
  const captureButton = document.getElementById('capture');

  const constraints = {
    video: true,
  };

  captureButton.addEventListener('click', () => {
    context.drawImage(player, 0, 0, canvas.width, canvas.height);

    <strong>// Stop all video streams.
    player.srcObject.getVideoTracks().forEach(track => track.stop());</strong>
  });

  navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
      // Attach the video stream to the video element and autoplay.
      player.srcObject = stream;
    });
&lt;/script>
</pre>

### Ask permission to use camera responsibly

If the user has not previously granted your site access to the camera then the instant that you call `getUserMedia()` the browser will prompt the user to grant your site permission to the camera.

Users hate getting prompted for access to powerful devices on their machine and they will frequently block the request, or they will ignore it if they don't understand the context for which the prompt has been created. It is best practice to only ask to access the camera when first needed. Once the user has granted access they won't be asked again. However, if the user rejects access, you can't get access again, unless they manually change camera permission settings.

Warning: Asking for access to the camera on page load will result in most of your users rejecting access to it.

## 互換性

More information about mobile and desktop browser implementation:

- [srcObject](https://www.chromestatus.com/feature/5989005896187904)
- [navigator.mediaDevices.getUserMedia()](https://www.chromestatus.com/features/5755699816562688)

We also recommend using the [adapter.js](https://github.com/webrtc/adapter) shim to protect apps from WebRTC spec changes and prefix differences.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}