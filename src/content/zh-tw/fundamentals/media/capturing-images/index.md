project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description:大多數瀏覽器都可訪問用戶的攝像頭。

{# wf_updated_on:2016-08-22 #} {# wf_published_on:2016-08-23 #}

# 採集用戶的圖像 {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

許多瀏覽器現在都能訪問用戶的視頻和音頻輸入。 不過，根據瀏覽器的不同，這一功能可能體現爲一種全動態的內置體驗，也可能通過授權給用戶設備上的其他應用來實現。

## 從簡單做起，循序漸進

最簡易的做法是直接要求用戶提供預先錄製的文件。 其實現步驟是：創建一個簡單的文件輸入元素，然後添加一個表示我們只能接受圖像文件的 `accept` 過濾器，在理想的情況下，我們可以直接從攝像頭獲取這些文件。

### 採集單個幀

此方法在所有平臺上都有效。在桌面平臺上，它會提示用戶通過文件系統上傳圖像文件。 在 iOS 上的 Safari 中，此方法會打開攝像頭應用以便您採集圖像，然後將其傳回網頁；在 Android 上，此方法允許用戶選擇使用哪一個應用來採集圖像，採集完畢後將其傳回網頁。

然後可將數據附加到一個 `<form>`，或通過 JavaScript 操作數據：偵聽 input 元素上的 `onchange` 事件，然後讀取事件 `target` 的 `files` 屬性。

### 獲得對攝像頭的訪問權

獲得對圖像文件的訪問權很簡單。

    <input type="file" accept="image/*" capture>
    

獲得對文件的訪問權後，便可隨意對其執行任何操作。例如，可以執行以下操作：

![](images/ios-chooser.png) ![](images/android-chooser.png)

現代瀏覽器可直接訪問攝像頭，我們可以藉此打造與網頁完全集成的體驗，讓用戶永遠都不需要離開瀏覽器。

<pre class="prettyprint">&lt;input type="file" accept="image/*" id="file-input">
&lt;script>
  const fileInput = document.getElementById('file-input');

  fileInput.addEventListener('change', (e) => doSomethingWithFiles(e.target.files));
&lt;/script>
</pre>

Warning: 直接訪問攝像頭是一項強大功能，需要徵得用戶的同意，並且網站需要託管在安全來源 (HTTPS) 上。

我們可以利用 WebRTC 規範中名爲 `getUserMedia()` 的 API 直接訪問攝像頭。 此時系統將提示用戶授予對其相連麥克風和攝像頭的訪問權。

    <video id="player" controls autoplay></video>
    <script>  
      var player = document.getElementById('player');
    
      var handleSuccess = function(stream) {
        player.srcObject = stream;
      };
    
      navigator.mediaDevices.getUserMedia({video: true})
          .then(handleSuccess);
    </script>
    

如果授權成功，該 API 將返回一個 `MediaStream`，其中包含來自攝像頭的數據，然後我們可以將數據附加到一個 `<video>` 元素、播放它以顯示實時預覽或將其附加到一個 `<canvas>` 以獲取快照。

要從攝像頭獲取數據，我們只需在傳遞給 `getUserMedia()` API 的約束對象中設置 `video: true`。

### 從攝像頭獲取快照

這段代碼本身的用處並不大。我們所能做的就是獲取視頻數據並進行播放。

要從攝像頭獲取原始數據，我們需要獲取 `getUserMedia()` 創建的卡片信息流，然後處理數據。 不同於 `Web Audio`，並沒有專用的卡片信息流處理 API 可用來處理網絡視頻，因此我們需要稍微用點歪招才能從用戶的攝像頭採集快照。

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

操作流程如下：

沒問題。

將來自攝像頭的數據存儲在 canvas 對象中後，就可以對其進行多種處理。 您可以：

### 不需要時停止從攝像頭流式傳輸視頻

最好在不再需要時停止使用攝像頭。 這樣做不僅可以節約電池電量和處理能力，還能增加用戶對應用的信心。

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

要停止訪問攝像頭，只需在 `getUserMedia()` 返回的卡片信息流的每個視頻磁軌上調用 `stop()`。

如果用戶之前未授予網站對攝像頭的訪問權，則調用 `getUserMedia` 時瀏覽器會立即提示用戶授予網站對攝像頭的訪問權。

用戶討厭在其機器上收到索要功能強大設備訪問權的提示，他們常常會屏蔽權限請求，而如果他們不瞭解提示的產生環境，也會將其忽略。最好的做法是在首次需要權限時只請求訪問攝像頭。 一旦用戶授予了訪問權，就不會再次收到提示。 但如果用戶拒絕授權，您就無法再次獲得訪問權，除非他們手動更改攝像頭權限設置。

### Handling a FileList object

Warning: 在頁面加載時請求獲得對攝像頭的訪問權將導致大多數用戶拒絕您訪問攝像頭。

有關移動和桌面瀏覽器實現的更多信息：

我們還建議使用 [adapter.js](https://github.com/webrtc/adapter) shim 來防止應用受到 WebRTC 規範變更和前綴差異的影響。

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

- 將其直接附加到一個 `<canvas>` 元素，這樣便能對其進行操作
- 將其下載至用戶的設備
- 通過將其附加到一個 `XMLHttpRequest`，上傳至服務器

## 以交互方式訪問攝像頭

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

1. 創建一個 canvas 對象，用來容納來自攝像頭的圖幀
2. 獲得對攝像頭卡片信息流的訪問權
3. 將其附加到一個 video 元素
4. 如果想精確地採集某一幀，可以利用 `drawImage()` 將 video 元素中的數據添加到 canvas 對象。

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

- 將其直接上傳至服務器
- 將其存儲在本地
- 對圖像應用好玩的特效

## 以負責任的方式請求攝像頭使用權限

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

## 兼容性

More information about mobile and desktop browser implementation:

- [srcObject](https://www.chromestatus.com/feature/5989005896187904)
- [navigator.mediaDevices.getUserMedia()](https://www.chromestatus.com/features/5755699816562688)

We also recommend using the [adapter.js](https://github.com/webrtc/adapter) shim to protect apps from WebRTC spec changes and prefix differences.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}