project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description:在此代码实验室中，您将构建一个 Progressive Web App，该应用即使在不可靠网络上也能快速加载、拥有桌面图标，并且可采用顶层全屏体验的方式加载。

{# wf_updated_on:2017-01-05 #} {# wf_published_on:2016-01-01 #}

# 您的第一个 Progressive Web App {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

## 简介

[Progressive Web App](/web/progressive-web-apps) 带来的体验将网络之长与应用之长相结合。用户在浏览器标签中第一次访问时就能体会到它们的好处，因为不需要进行任何安装。在用户随着时间的推移增进与应用的关系后，其功能会变得越来越强大。它即使在不可靠网络上也能快速加载、能够发送相关推送通知、具有桌面图标，并且可采用顶层全屏体验的方式加载。

### 什么是 Progressive Web App？

Progressive Web App 具备以下特点：

* **渐进式** - 适用于选用任何浏览器的所有用户，因为它是以渐进式增强作为核心宗旨来开发的。
* **自适应** - 适合任何机型：桌面设备、移动设备、平板电脑或任何未来设备。
* **连接无关性** - 能够借助于服务工作线程在离线或低质量网络状况下工作。
* **类似应用** - 由于是在 App Shell 模型基础上开发，因此具有应用风格的交互和导航，给用户以应用般的熟悉感。
* **持续更新** - 在服务工作线程更新进程的作用下时刻保持最新状态。
* **安全** - 通过 HTTPS 提供，以防止窥探和确保内容不被篡改。
* **可发现** - W3C 清单和服务工作线程注册作用域能够让搜索引擎找到它们，从而将其识别为“应用”。
* **可再互动** - 通过推送通知之类的功能简化了再互动。
* **可安装** - 用户可免去使用应用商店的麻烦，直接将对其最有用的应用“保留”在主屏幕上。
* **可链接** - 可通过网址轻松分享，无需复杂的安装。

此代码实验室将引导您逐步创建自己的 Progressive Web App，包括设计考虑事项，以及旨在确保您的应用符合 Progressive Web App 关键原则的实现细节。<aside class="key-point">

<p>Looking for more? Check out the talks from the  <a href="https://www.youtube.com/playlist?list=PLNYkxOF6rcIAWWNR_Q6eLPhsyx6VvYjVb">2016 Progressive Web App Summit</a>.</p>

</aside> 

### 我们将要开发什么应用？

<table>
  <p>
    <tr>
      <td colspan="1" rowspan="1">
        </p>

<p>In this codelab, you're going to build a Weather web app using Progressive Web App techniques. Your app will:</p>

        
        <ul>
          
<li>Utilize and demonstrate the above principles of Progressive Web Apps.</li>
<li>Use live weather data.</li>
          
          <li>
            
<p>Provide app-like interactions to allow the user to add cities.</p>
</td><td colspan="1" rowspan="1">
              </li> </ul>

<p><img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png"></p>

              
              <p>
                </td> </tr>
              </p></table> 
              
              <h3>
                您将学习的内容
              </h3>
              
              <ul>
                <li>
                  <strong>渐进式</strong> - 我们将自始至终使用渐进式增强。
                </li>
                <li>
                  <strong>自适应</strong> - 我们将确保它适合任何机型。
                </li>
                <li>
                  <strong>连接</strong>无关性 - 我们将通过服务工作线程缓存 App Shell。
                </li>
              </ul>
              
              <h3>
                您需具备的条件
              </h3>
              
              <ul>
                <li>
                  如何利用“App Shell”方法设计和构建应用
                </li>
                <li>
                  如何让您的应用能够离线工作
                </li>
                <li>
                  <a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip">The sample code</a>
                </li>
                <li>
                  A text editor
                </li>
                <li>
                  Basic knowledge of HTML, CSS, JavaScript, and <a href="https://developer.chrome.com/devtools">Chrome DevTools</a>
                </li>
              </ul>
              
              <p>
                在此代码实验室中，您将会利用 Progressive Web App 技术来开发一款天气网络应用。 我们来看看 Progressive Web App 的属性：
              </p>
              
              <h2>
                设置
              </h2>
              
              <h3>
                下载代码
              </h3>
              
              <p>
                此代码实验室重点介绍 Progressive Web App。这里不详细介绍无关的概念和代码块，也不提供这些内容供您简单复制和粘贴。
              </p>
              
              <p>
                <a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip">Download source code</a>
              </p>
              
              <p>
                Unpack the downloaded zip file. This will unpack a root folder (<code>your-first-pwapp-master</code>), which contains one folder for each step of this codelab, along with all of the resources you will need.
              </p>
              
              <p>
                解压下载的 zip 文件。这将解压根文件夹 (<code>your-first-pwapp-master</code>)，其中包含此代码实验室的每个步骤的对应文件夹，以及您需要的所有资源。
              </p>
              
              <h3>
                安装并验证网络服务器
              </h3>
              
              <p>
                <code>step-NN</code> 文件夹包含此代码实验室的每个步骤所需的结束状态。这些文件夹供您参考。我们将在一个名为 <code>work</code> 的目录中完成所有的编码工作。
              </p>
              
              <p>
                <a href="https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb">Install Web Server for Chrome</a>
              </p>
              
              <p>
                After installing the Web Server for Chrome app, click on the Apps shortcut on the bookmarks bar:
              </p>
              
              <p>
                <img src="img/9efdf0d1258b78e4.png" alt="9efdf0d1258b78e4.png" />
              </p><aside class="key-point">

<p>More help:  <a href="https://support.google.com/chrome_webstore/answer/3060053">Add and open Chrome apps</a></p>

</aside> 
              
              <p>
                In the ensuing window, click on the Web Server icon:
              </p>
              
              <p>
                <img src="img/dc07bbc9fcfe7c5b.png" alt="dc07bbc9fcfe7c5b.png" />
              </p>
              
              <p>
                You'll see this dialog next, which allows you to configure your local web server:
              </p>
              
              <p>
                <img src="img/433870360ad308d4.png" alt="433870360ad308d4.png" />
              </p>
              
              <p>
                Click the <strong>choose folder</strong> button, and select the <code>work</code> folder. This will enable you to serve your work in progress via the URL highlighted in the web server dialog (in the <strong>Web Server URL(s)</strong> section).
              </p>
              
              <p>
                点击 <strong>choose folder</strong> 按钮，然后选择 <code>work</code> 文件夹。这样您就可以通过网络服务器对话框（在 <strong>Web Server URL(s)</strong> 部分）中突出显示的网址为正在进行的工作提供支持。
              </p>
              
              <p>
                <img src="img/39b4e0371e9703e6.png" alt="39b4e0371e9703e6.png" />
              </p>
              
              <p>
                Then stop and restart the server by sliding the toggle labeled "Web Server: STARTED" to the left and then back to the right.
              </p>
              
              <p>
                <img src="img/daefd30e8a290df5.png" alt="daefd30e8a290df5.png" />
              </p>
              
              <p>
                Now visit your work site in your web browser (by clicking on the highlighted Web Server URL) and you should see a page that looks like this:
              </p>
              
              <p>
                <img src="img/aa64e93e8151b642.png" alt="aa64e93e8151b642.png" />
              </p>
              
              <p>
                This app is not yet doing anything interesting - so far, it's just a minimal skeleton with a spinner we're using to verify your web server functionality. We'll add functionality and UI features in subsequent steps.
              </p><aside class="key-point">

<p>From this point forward, all testing/verification (e.g. the<strong> Test It Out</strong> sections in subsequent steps) should be performed using this web server setup.</p>

</aside> 
              
              <h2>
                构建 App Shell
              </h2>
              
              <h3>
                什么是 App Shell？
              </h3>
              
              <p>
                显然，此应用还没有做任何有趣的事情 - 目前，它只是一个最小的主干，我们使用其中的微调框验证网络服务器的功能。我们会在接下来的步骤中添加功能和 UI 功能。
              </p>
              
              <p>
                App Shell 是驱动 Progressive Web App 用户界面所需的最小的 HTML、CSS 和 JavaScript，是确保获得可靠而又出色性能的组件之一。它的第一次加载速度非常快，并且能够立即缓存。“缓存”意味着 Shell 文件一旦通过网络完成加载，就会保存到本地设备中。以后每当用户打开应用时，就会自动从本地设备的缓存中打开 Shell 文件，这样应用就能超快启动。
              </p>
              
              <p>
                App Shell 架构将核心应用基础架构和 UI 与数据分离。所有 UI 和基础架构都利用服务工作线程缓存在本地，这样在后续加载时，Progressive Web App 只需检索必要的数据，而不必加载所有内容。
              </p>
              
              <p>
                <img src="img/156b5e3cc8373d55.png" alt="156b5e3cc8373d55.png" />
              </p>
              
              <p>
                换个说法，App Shell 就类似于您在开发本机应用时需要向应用商店发布的一组代码。它是让您的应用成功起步所需的核心组件，但可能并不包含数据。
              </p>
              
              <h3>
                为何采用 App Shell 架构？
              </h3>
              
              <p>
                采用 App Shell 架构可令您专注于提高速度，从而赋予您的 Progressive Web App 类似于本机应用的特性：即时加载和定期更新，并且根本无需借助应用商店。
              </p>
              
              <h3>
                设计 App Shell
              </h3>
              
              <p>
                第一步是将设计细分成其核心组件。
              </p>
              
              <p>
                问问自己：
              </p>
              
              <ul>
                <li>
                  Chrome 52 或更高版本
                </li>
                <li>
                  <a href="https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb">Web Server for Chrome</a>，或者使用您自己选择的网络服务器
                </li>
                <li>
                  示例代码
                </li>
              </ul>
              
              <p>
                我们将创建一个天气应用，将其作为我们的第一个 Progressive Web App。其关键组件将包括：
              </p>
              
              <table>
                <p>
                  <tr>
                    <td colspan="1" rowspan="1">
                      </p> 
                      
                      <ul>
                        
<li>Header with a title, and add/refresh buttons</li>
<li>Container for forecast cards</li>
<li>A forecast card template</li>
<li>A dialog box for adding new cities</li>
                        
                        <li>
                          
<p>A loading indicator</p>
</td><td colspan="1" rowspan="1">
                            </li> </ul>

<p><img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png"></p>

                            
                            <p>
                              </td> </tr>
                            </p></table> 
                            
                            <p>
                              设计更复杂的应用时，首次加载所不需要的内容可稍后请求，然后缓存起来以供日后使用。例如，我们可以将“New City”对话框的加载推迟到完成了首次加载体验并且有一些空闲周期之时。
                            </p>
                            
                            <h2>
                              实现 App Shell
                            </h2>
                            
                            <p>
                              启动任何项目都有多种方法，我们一般建议使用 Web Starter Kit。但在此情况下，为使我们的项目尽可能简单并侧重于 Progressive Web App，我们为您提供了所需的全部资源。
                            </p>
                            
                            <h3>
                              为 App Shell 创建 HTML
                            </h3>
                            
                            <p>
                              现在，我们将添加在<a href="/web/fundamentals/getting-started/your-first-progressive-web-app/step-01">构建 App Shell</a>中讨论过的核心组件。
                            </p>
                            
                            <p>
                              切记，关键组件将包括：
                            </p>
                            
                            <ul>
                              <li>
                                哪些内容需要立即呈现在屏幕上？
                              </li>
                              <li>
                                还有哪些其他 UI 组件对我们的应用很重要？
                              </li>
                              <li>
                                App Shell 需要哪些支持资源？例如图像、JavaScript、样式等。
                              </li>
                              <li>
                                A dialog for adding new cities
                              </li>
                              <li>
                                A loading indicator
                              </li>
                            </ul>
                            
                            <p>
                              <code>work</code> 目录中已有的 <code>index.html</code> 文件类似于以下内容（这是实际内容的子集，请不要将此代码复制到您的文件中）：
                            </p>
                            
                            <pre><code>&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;meta charset="utf-8"&gt;
  &lt;meta http-equiv="X-UA-Compatible" content="IE=edge"&gt;
  &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
  &lt;title&gt;Weather PWA&lt;/title&gt;
  &lt;link rel="stylesheet" type="text/css" href="styles/inline.css"&gt;
&lt;/head&gt;
&lt;body&gt;
  &lt;header class="header"&gt;
    &lt;h1 class="header__title"&gt;Weather PWA&lt;/h1&gt;
    &lt;button id="butRefresh" class="headerButton"&gt;&lt;/button&gt;
    &lt;button id="butAdd" class="headerButton"&gt;&lt;/button&gt;
  &lt;/header&gt;

  &lt;main class="main"&gt;
    &lt;div class="card cardTemplate weather-forecast" hidden&gt;
    . . .
    &lt;/div&gt;
  &lt;/main&gt;

  &lt;div class="dialog-container"&gt;
  . . .
  &lt;/div&gt;

  &lt;div class="loader"&gt;
    &lt;svg viewBox="0 0 32 32" width="32" height="32"&gt;
      &lt;circle id="spinner" cx="16" cy="16" r="14" fill="none"&gt;&lt;/circle&gt;
    &lt;/svg&gt;
  &lt;/div&gt;

  &lt;!-- Insert link to app.js here --&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>
                            
                            <p>
                              请注意，默认情况下，加载器可见。这可以确保用户在网页加载时立即看到加载器，从而让他们清楚地知道，内容正在加载。
                            </p>
                            
                            <p>
                              为节省时间，我们已经创建了样式表供您使用。
                            </p><aside class="key-point">

<p>We've given you the markup and styles to save you some time and make sure you're starting on a solid foundation. In the next section, you'll have an opportunity to write your own code.</p>

</aside> 
                            
                            <h3>
                              连接关键 JavaScript 应用代码
                            </h3>
                            
                            <p>
                              既然 UI 已大部分准备就绪，现在可以着手连接代码，让一切运转起来。与 App Shell 的其余部分一样，对于哪些代码需要成为关键体验的组成部分，以及哪些代码可以延后加载，要有清醒的认识。
                            </p>
                            
                            <p>
                              您的工作目录同时已包含应用代码 (<code>scripts/app.js</code>)，您将在其中找到以下内容：
                            </p>
                            
                            <ul>
                              <li>
                                标头，其中包含标题和添加/刷新按钮
                              </li>
                              <li>
                                预报卡片容器
                              </li>
                              <li>
                                预报卡片模板
                              </li>
                              <li>
                                用于添加新城市的对话框
                              </li>
                              <li>
                                加载指示器
                              </li>
                              <li>
                                Some fake data (<code>initialWeatherForecast</code>) you can use to quickly test how things render.
                              </li>
                            </ul>
                            
                            <h3>
                              测试
                            </h3>
                            
                            <p>
                              既然您已添加了核心 HTML、样式和 JavaScript，是时候试用一下应用了。
                            </p>
                            
                            <p>
                              为了了解虚假天气数据是如何渲染的，请取消对您的 <code>index.html</code> 文件底部的以下行的注释：
                            </p>
                            
                            <pre><code>&lt;!--&lt;script src="scripts/app.js" async&gt;&lt;/script&gt;--&gt;
</code></pre>
                            
                            <p>
                              接下来，取消对您的 <code>app.js</code> 文件底部的以下行的注释：
                            </p>
                            
                            <pre><code>// app.updateForecastCard(initialWeatherForecast);
</code></pre>
                            
                            <p>
                              重新加载您的应用。结果会是格式完美（尽管虚假，您可以按日期分辨）预报卡片，其中已禁用微调框，如下所示：
                            </p>
                            
                            <p>
                              <img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-04/">链接</a>
                            </p>
                            
                            <p>
                              对其进行测试并验证它可以如期工作后，您就可以重新使用虚假数据移除对 <code>app.updateForecastCard</code> 的调用了。我们只有在确保所有事情都能如期工作时才需要它。
                            </p>
                            
                            <h2>
                              从快速首次加载开始
                            </h2>
                            
                            <p>
                              Progressive Web App 应启动迅速，并且立即就能使用。在目前状态下，我们的天气应用虽启动迅速，却无法使用，因为没有任何数据。我们可以发出 AJAX 请求来获取该数据，但那会额外增加一个请求，使首次加载时间变得更长。可以改为在首次加载时提供真实数据。
                            </p>
                            
                            <h3>
                              注入天气预报数据
                            </h3>
                            
                            <p>
                              在此代码实验室中，我们将模拟服务器将天气预报直接注入 JavaScript 中，但在生产应用中，最新天气预报数据将由服务器根据用户的 IP 地址地理定位进行注入。
                            </p>
                            
                            <p>
                              代码已经包含我们即将注入的数据。这就是我们之前的步骤中使用的 <code>initialWeatherForecast</code>。
                            </p>
                            
                            <h3>
                              区分首次运行
                            </h3>
                            
                            <p>
                              但我们如何知道何时显示这些信息，因为未来加载时天气应用将从缓存中获取，届时这些信息可能不再相关？如果用户在后续访问时加载应用，他们可能已改换城市，因此我们需要加载的是这些城市的信息，而不一定是他们查询过的第一个城市。
                            </p>
                            
                            <p>
                              用户首选项（比如用户订阅的城市列表）应利用 IndexedDB 或其他快速存储机制存储在本地。为尽可能简化此代码实验室，我们使用了 <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage">localStorage</a>，但它并非生产应用的理想选择，因为它是一种封闭的同步存储机制，在某些设备上可能会运行得非常缓慢。
                            </p><aside class="key-point">

<p><strong>Extra Credit</strong>: Replace <code>localStorage</code> implementation with  <a href="https://www.npmjs.com/package/idb">idb</a>, check out  <a href="https://github.com/localForage/localForage">localForage</a> as a simple wrapper to idb.</p>

</aside> 
                            
                            <p>
                              首先，让我们添加必要的代码来保存用户首选项。请在您的代码中找到以下 TODO 注释。
                            </p>
                            
                            <pre><code>  // TODO add saveSelectedCities function here
</code></pre>
                            
                            <p>
                              并在注释下方添加以下代码。
                            </p>
                            
                            <pre><code>  // Save list of cities to localStorage.
  app.saveSelectedCities = function() {
    var selectedCities = JSON.stringify(app.selectedCities);
    localStorage.selectedCities = selectedCities;
  };
</code></pre>
                            
                            <p>
                              接下来，让我们添加启动代码，以检查用户是否保存了任何城市并渲染这些城市，或使用注入的数据。找到以下注释：
                            </p>
                            
                            <pre><code>  // TODO add startup code here
</code></pre>
                            
                            <p>
                              并在注释下方添加以下代码：
                            </p>
                            
                            <pre><code>/************************************************************************
   *

   * Code required to start the app
   *
   * NOTE:To simplify this codelab, we've used localStorage.
   *   localStorage is a synchronous API and has serious performance
   *   implications.It should not be used in production applications!
   *   Instead, check out IDB (https://www.npmjs.com/package/idb) or
   *   SimpleDB (https://gist.github.com/inexorabletash/c8069c042b734519680c)
   ************************************************************************/

  app.selectedCities = localStorage.selectedCities;
  if (app.selectedCities) {
    app.selectedCities = JSON.parse(app.selectedCities);
    app.selectedCities.forEach(function(city) {
      app.getForecast(city.key, city.label);
    });
  } else {
    /* The user is using the app for the first time, or the user has not
     * saved any cities, so show the user some fake data.A real app in this
     * scenario could guess the user's location via IP lookup and then inject
     * that data into the page.
     */
    app.updateForecastCard(initialWeatherForecast);
    app.selectedCities = [
      {key: initialWeatherForecast.key, label: initialWeatherForecast.label}
    ];
    app.saveSelectedCities();
  }
</code></pre>
                            
                            <p>
                              启动代码会检查本地存储中是否存储了任何城市。如果存储了城市，它就会解析本地存储数据，然后为保存的每个城市显示预报卡片。如果没有存储城市，启动代码会使用虚假的预报数据，并将其保存为默认城市。
                            </p>
                            
                            <h3>
                              保存所选城市
                            </h3>
                            
                            <p>
                              最后，您需要修改“Add City”按钮处理程序，以便将所选的城市添加到本地存储。
                            </p>
                            
                            <p>
                              更新您的 <code>butAddCity</code> 点击处理程序，以便匹配以下代码：
                            </p>
                            
                            <pre><code>document.getElementById('butAddCity').addEventListener('click', function() {
    // Add the newly selected city
    var select = document.getElementById('selectCityToAdd');
    var selected = select.options[select.selectedIndex];
    var key = selected.value;
    var label = selected.textContent;
    if (!app.selectedCities) {
      app.selectedCities = [];
    }
    app.getForecast(key, label);
    app.selectedCities.push({key: key, label: label});
    app.saveSelectedCities();
    app.toggleAddDialog(false);
  });
</code></pre>
                            
                            <p>
                              新添加的内容是 <code>app.selectedCities</code> 的初始化（如果他不存在），以及对 <code>app.selectedCities.push()</code> 和 <code>app.saveSelectedCities()</code> 的调用。
                            </p>
                            
                            <h3>
                              测试
                            </h3>
                            
                            <ul>
                              <li>
                                标头，其中包含标题和添加/刷新按钮
                              </li>
                              <li>
                                预报卡片容器
                              </li>
                              <li>
                                预报卡片模板
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-05/">链接</a>
                            </p>
                            
                            <h2>
                              利用服务工作线程预缓存 App Shell
                            </h2>
                            
                            <p>
                              Progressive Web App 必须快速并且可安装，这意味着它们能够在在线、离线以及间歇性、慢速连接下工作。要实现此目标，我们需要利用服务工作线程缓存我们的 App Shell，让其始终都能快速而又可靠地投入使用。
                            </p>
                            
                            <p>
                              如果您不熟悉服务工作线程，可以通过阅读有关其作用、生命周期等内容的<a href="/web/fundamentals/primers/service-worker/">服务工作线程简介</a>来获取基本的了解。完成此代码实验室后，请务必查看<a href="https://goo.gl/jhXCBy">调试服务工作线程代码实验室</a>，了解有关如何使用服务工作线程的详细信息。
                            </p>
                            
                            <p>
                              应将通过服务工作线程提供的功能视为渐进式增强功能，仅在得到浏览器支持时添加。例如，您可以通过为您的应用缓存 App Shell 和数据，使其即便在网络不可用时仍然可用。不支持服务工作线程时，不会调用离线代码，用户只能获得基础体验。利用功能检测来提供渐进式增强功能的开销很小，在不支持该功能的旧浏览器上它也不会中断运行。
                            </p><aside class="key-point">

<p><strong>Remember</strong>: Service worker functionality is only available on pages that are accessed via HTTPS (<a href="http://localhost">http://localhost</a> and equivalents will also work, to facilitate testing). To learn about the rationale behind this restriction check out  <a href="http://www.chromium.org/Home/chromium-security/prefer-secure-origins-for-powerful-new-features">Prefer Secure Origins For Powerful New Features</a> from the Chromium team.</p>

</aside> 
                            
                            <h3>
                              在服务工作线程可用时注册它
                            </h3>
                            
                            <p>
                              使应用离线工作的第一步是注册一个服务工作线程，即一个无需打开网页或用户交互便可实现后台功能的脚本。
                            </p>
                            
                            <p>
                              注册需要执行两个简单的步骤：
                            </p>
                            
                            <ol start="1">
                              <li>
                                让浏览器将 JavaScript 文件注册为服务工作线程。
                              </li>
                              
                              <li>
                                创建包含此服务工作线程的 JavaScript 文件。
                              </li>
                            </ol>
                            
                            <p>
                              首先，我们需要检查浏览器是否支持服务工作线程，如果支持，则注册服务工作线程。将以下代码添加到 <code>app.js</code>（在 <code>// TODO add service worker code here</code> 注释之后）：
                            </p>
                            
                            <pre><code>  if ('serviceWorker' in navigator) {
    navigator.serviceWorker
             .register('./service-worker.js')
             .then(function() { console.log('Service Worker Registered'); });
  }
</code></pre>
                            
                            <h3>
                              缓存网站资产
                            </h3>
                            
                            <p>
                              注册服务工作线程后，用户首次访问页面时将会触发安装事件。在此事件处理程序内，我们将缓存应用所需的全部资产。
                            </p><aside class="warning">

<p>The code below must NOT be used in production, it covers only the most basic use cases and it's easy to get yourself into a state where your app shell will never update. Be sure to review the section below that discusses the pitfalls of this implementation and how to avoid them.</p>

</aside> 
                            
                            <p>
                              触发服务工作线程时，它应打开 <a href="https://developer.mozilla.org/en-US/docs/Web/API/Cache">caches</a> 对象并为其填充加载 App Shell 所需的资产。在应用的根文件夹（应为 <code>your-first-pwapp-master/work</code> 目录）中创建名为 <code>service-worker.js</code> 的文件。这个文件之所以必须位于应用的根文件夹内，是因为服务工作线程的作用域由该文件所在的目录定义。将此代码添加到新建的 <code>service-worker.js</code> 文件中：
                            </p>
                            
                            <pre><code>var cacheName = 'weatherPWA-step-6-1';
var filesToCache = [];

self.addEventListener('install', function(e) {
  console.log('[ServiceWorker] Install');
  e.waitUntil(
    caches.open(cacheName).then(function(cache) {
      console.log('[ServiceWorker] Caching app shell');
      return cache.addAll(filesToCache);
    })
  );
});
</code></pre>
                            
                            <p>
                              首先，我们需要通过 <code>caches.open()</code> 打开缓存并提供一个缓存名称。提供缓存名称可让我们对文件进行版本控制，或将数据与 App Shell 分开，以便我们能轻松地更新某个数据，而不会影响其他数据。
                            </p>
                            
                            <p>
                              缓存打开后，我们便可调用 <code>cache.addAll()</code>，这个带有网址列表参数的方法随即从服务器获取文件，并将响应添加到缓存内。遗憾的是，<code>cache.addAll()</code> 具有原子性，如果任何一个文件失败，整个缓存步骤也将失败！
                            </p>
                            
                            <p>
                              好的，让我们开始了解如何使用 DevTools 了解和调试服务工作线程。重新加载页面之前，打开 DevTools，转至 __Application __面板的 __Service Worker __窗格。它看上去应该像下面这样。
                            </p>
                            
                            <p>
                              <img src="img/ed4633f91ec1389f.png" alt="ed4633f91ec1389f.png" />
                            </p>
                            
                            <p>
                              如果您看到类似于这样的空白页面，就表示当前打开的页面没有注册服务工作线程。
                            </p>
                            
                            <p>
                              现在，重新加载页面。Service Worker 窗格现在应该像下面这样。
                            </p>
                            
                            <p>
                              <img src="img/bf15c2f18d7f945c.png" alt="bf15c2f18d7f945c.png" />
                            </p>
                            
                            <p>
                              如果您看到类似的信息，就表示页面正在运行服务工作线程。
                            </p>
                            
                            <p>
                              现在，我们简要插入一点其他内容，说明您在开发服务工作线程时可能会遇到的 Gotcha。为了说明这一问题，让我们在 <code>service-worker.js</code> 文件中的 <code>install</code> 侦听器下面添加 <code>activate</code> 事件侦听器。
                            </p>
                            
                            <pre><code>self.addEventListener('activate', function(e) {
  console.log('[ServiceWorker] Activate');
});
</code></pre>
                            
                            <p>
                              <code>activate</code> 事件会在服务工作线程启动时触发。
                            </p>
                            
                            <p>
                              打开 DevTools Console，重新加载页面，切换到 Application 面板中的 Service Worker 窗格，然后点击激活的服务工作线程上的 Inspect。您预期会看到 <code>[ServiceWorker] Activate</code> 消息记录到控制台，但并没有发生。请查看 Service Worker 窗格，您会看到新的服务工作线程（包含激活事件侦听器）处于“等待”状态。
                            </p>
                            
                            <p>
                              <img src="img/1f454b6807700695.png" alt="1f454b6807700695.png" />
                            </p>
                            
                            <p>
                              从根本上说，只要页面有打开的标签，以前的服务工作线程就会继续控制页面。因此，您 <em>可以</em> 关闭然后重新打开页面，或者按 __skipWaiting __按钮，但是更长期的解决方案只需启用 DevTools 的 Service Worker 窗格上的 __Update on Reload __复选框。如果启用此复选框，服务工作线程会在每次页面重新加载时强制更新。
                            </p>
                            
                            <p>
                              现在启用 __update on reload __复选框，并重新加载页面，以确认新的服务工作线程被激活。
                            </p>
                            
                            <p>
                              <strong>Note: __ 您可能会在 Application 面板的 Service Worker 窗格中发到一个错误（类似于以下内容），</strong>完全可以__忽略此错误。
                            </p>
                            
                            <p>
                              <img src="img/b1728ef310c444f5.png" alt="b1728ef310c444f5.png" />
                            </p>
                            
                            <p>
                              有关在 DevTools 中检查和调试服务工作线程的内容结束。我们稍后将向您展示更多技巧。让我们继续开发您的应用。
                            </p>
                            
                            <p>
                              我们来展开 <code>activate</code> 事件侦听器，添加以下逻辑来更新缓存。更新您的代码以匹配以下代码。
                            </p>
                            
                            <pre><code>self.addEventListener('activate', function(e) {
  console.log('[ServiceWorker] Activate');
  e.waitUntil(
    caches.keys().then(function(keyList) {
      return Promise.all(keyList.map(function(key) {
        if (key !== cacheName) {
          console.log('[ServiceWorker] Removing old cache', key);
          return caches.delete(key);
        }
      }));
    })
  );
  return self.clients.claim();
});
</code></pre>
                            
                            <p>
                              此代码可以确保您的服务工作线程在任何 App Shell 文件更改时更新其缓存。为了使其工作，您需要在服务工作线程文件的顶部增加 <code>cacheName</code> 变量。
                            </p>
                            
                            <p>
                              最后一句修复了您可能会在以下（可选）信息框中读取的极端情况。
                            </p><aside class="key-point">

<p>When the app is complete, <code>self.clients.claim()</code> fixes a corner case in which the app wasn't returning the latest data. You can reproduce the corner case by commenting out the line below and then doing the following steps: 1) load app for first time so that the initial New York City data is shown 2) press the refresh button on the app 3) go offline 4) reload the app. You expect to see the newer NYC data, but you actually see the initial data. This happens because the service worker is not yet activated. <code>self.clients.claim()</code> essentially lets you activate the service worker faster.</p>

</aside> 
                            
                            <p>
                              最后，让我们更新 App Shell 所需的文件列表。在数组中，我们需要加入应用所需的全部文件，包括图像、JavaScript、样式表等。在接近 <code>service-worker.js</code> 文件顶部的地方，使用以下代码替换 <code>var filesToCache = [];</code>：
                            </p>
                            
                            <pre><code>var filesToCache = [
  '/',
  '/index.html',
  '/scripts/app.js',
  '/styles/inline.css',
  '/images/clear.png',
  '/images/cloudy-scattered-showers.png',
  '/images/cloudy.png',
  '/images/fog.png',
  '/images/ic_add_white_24px.svg',
  '/images/ic_refresh_white_24px.svg',
  '/images/partly-cloudy.png',
  '/images/rain.png',
  '/images/scattered-showers.png',
  '/images/sleet.png',
  '/images/snow.png',
  '/images/thunderstorm.png',
  '/images/wind.png'
];
</code></pre><aside class="key-point">

<p>Be sure to include all permutations of file names, for example our app is served from <code>index.html</code>, but it may also be requested as <code>/</code> since the server sends <code>index.html</code> when a root folder is requested. You could deal with this in the <code>fetch</code> method, but it would require special casing which may become complex.</p>

</aside> 
                            
                            <p>
                              我们的应用现在还不能离线运行。我们虽已缓存了 App Shell 组件，但仍需从本地缓存加载它们。
                            </p>
                            
                            <h3>
                              从缓存提供 App Shell
                            </h3>
                            
                            <p>
                              服务工作线程提供了拦截 Progressive Web App 发出的请求并在服务工作线程内对它们进行处理的能力。这意味着我们可以决定想要如何处理请求，并可提供我们自己的已缓存响应。
                            </p>
                            
                            <p>
                              例如：
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(event) {
  // Do something interesting with the fetch here
});
</code></pre>
                            
                            <p>
                              现在让我们从缓存提供 App Shell。将以下代码添加到 <code>service-worker.js</code> 文件的底部：
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(e) {
  console.log('[ServiceWorker] Fetch', e.request.url);
  e.respondWith(
    caches.match(e.request).then(function(response) {
      return response || fetch(e.request);
    })
  );
});
</code></pre>
                            
                            <p>
                              <code>caches.match()</code> 会由内而外对触发<a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">抓取</a>事件的网络请求进行评估，并检查以确认它是否位于缓存内。它随即使用已缓存版本作出响应，或者利用 <code>fetch</code> 从网络获取一个副本。<code>response</code> 通过 <code>e.respondWith()</code> 传回至网页。
                            </p><aside class="warning">

<p>If you're not seeing the <code>[ServiceWorker]</code> logging in the console, be sure you've changed the <code>cacheName</code> variable and that you're inspecting the right service worker by opening the Service Worker pane in the Applications panel and clicking <strong>inspect</strong> on the running service worker. If that doesn't work, see the section on Tips for testing live service workers.</p>

</aside> 
                            
                            <h3>
                              测试
                            </h3>
                            
                            <p>
                              您的应用现在可以离线运行了！我们来试一试。
                            </p>
                            
                            <p>
                              重新加载页面，然后转至 DevTools 的 <strong>Application</strong> 面板的 <strong>Cache Storage</strong> 窗格。展开这个部分，您会看到左侧列出的 App Shell 缓存的名称。当您点击 App Shell 缓存后，可以看到它当前缓存的所有资源。
                            </p>
                            
                            <p>
                              <img src="img/ab9c361527825fac.png" alt="ab9c361527825fac.png" />
                            </p>
                            
                            <p>
                              现在，我们测试一下离线模式。返回到 DevTools 的 <strong>Service Worker</strong> 窗格，然后启用 <strong>Offline</strong> 复选框。启用后，您应看到 <strong>Network</strong> 面板标签旁边显示很小的黄色警告图标。这表示您处于离线状态。
                            </p>
                            
                            <p>
                              <img src="img/7656372ff6c6a0f7.png" alt="7656372ff6c6a0f7.png" />
                            </p>
                            
                            <p>
                              重新加载页面... 这样就可以了！至少差不多了。请注意它加载初始（虚假）天气数据的方式。
                            </p>
                            
                            <p>
                              <img src="img/8a959b48e233bc93.png" alt="8a959b48e233bc93.png" />
                            </p>
                            
                            <p>
                              请查看 <code>app.getForecast()</code> 中的 <code>else</code> 语句，了解应用能够加载虚假数据的原因。
                            </p>
                            
                            <p>
                              下一步就是修改应用和服务工作线程逻辑，以便能够缓存天气数据，并在应用处于离线状态时从缓存中返回最新数据。
                            </p>
                            
                            <p>
                              <strong>提示：</strong> 要开始刷新和清除已保存的所有数据（localStoarge、indexedDB 数据、缓存文件）以及移除任何服务工作线程，请使用 Application 标签中的 Clear storage 窗格。
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-06/">链接</a>
                            </p>
                            
                            <h3>
                              注意边缘情况
                            </h3>
                            
                            <p>
                              如前所述，由于存在许多未处理的边缘情况，这些代码<strong>不得在生产环境中使用</strong>。
                            </p>
                            
                            <h4>
                              缓存取决于为每次更改更新缓存键
                            </h4>
                            
                            <p>
                              例如，此缓存方法要求您在每一次内容发生更改时更新缓存键，否则将不会更新缓存，而将提供旧内容。因此，在处理项目时，请务必在每次发生内容更改时更改缓存键！
                            </p>
                            
                            <h4>
                              要求在每次发生更改时重新下载所有内容
                            </h4>
                            
                            <p>
                              另一个缺点是，每次文件发生更改时整个缓存都会失效，需要重新下载。这意味着，修正一个简单的单字符拼写错误都会使缓存失效，需要重新下载所有内容。根本没有任何效率可言。
                            </p>
                            
                            <h4>
                              浏览器缓存可能阻止服务工作线程缓存更新
                            </h4>
                            
                            <p>
                              这里还有一个重要注意事项。安装处理程序期间发出的 HTTPS 请求必须直接发送至网络，并且不从浏览器的缓存返回响应，这一点至关重要。否则，浏览器可能返回已缓存的旧版本，导致服务工作线程缓存实际上从未得到更新！
                            </p>
                            
                            <h4>
                              注意缓存优先策略在生产环境中的使用
                            </h4>
                            
                            <p>
                              我们的应用使用缓存优先策略，这会导致在返回任何已缓存内容的副本时都不会查询网络。尽管缓存优先策略易于实现，却可能在日后带来挑战。一旦缓存了主机页面和服务工作线程注册的副本，更改服务工作线程的配置可能变得极为困难（因为配置取决于其定义位置），并且您可能发现，自己部署的网站极难更新！
                            </p>
                            
                            <h4>
                              如何避免这些边缘情况？
                            </h4>
                            
                            <p>
                              那么我们如何避免这些边缘情况呢？使用 <a href="https://github.com/GoogleChrome/sw-precache">sw-precache</a> 之类的内容库，它能对过期内容实施精细控制，确保请求直接发送至网络，以及为您处理所有困难工作。
                            </p>
                            
                            <h3>
                              实时服务工作线程测试提示
                            </h3>
                            
                            <p>
                              调试服务工作线程可能是一项挑战，并且在涉及缓存的情况下，如果缓存未在您预期的时间进行更新，就可能变成一场更可怕的恶梦。典型服务工作线程生命周期和代码错误的双重夹击可能很快令您感到失望。但请不要失望。您可以利用一些工具来改善自己的处境。
                            </p>
                            
                            <h4>
                              开始刷新
                            </h4>
                            
                            <p>
                              在某些情况下，您可能会发现自己正在下载已缓存的数据或者内容没有如期更新。要清除已保存的所有数据（localStoarge、indexedDB 数据、缓存文件）以及移除任何服务工作线程，请使用 Application 标签中的 Clear storage 窗格。
                            </p>
                            
                            <p>
                              一些其他提示：
                            </p>
                            
                            <ul>
                              <li>
                                一个 <code>app</code> 对象，其中包含应用所需的一些关键信息。
                              </li>
                              <li>
                                标头中所有按钮 (<code>add/refresh</code>) 和“Add City”对话框中所有按钮 (<code>add/cancel</code>) 的事件侦听器。
                              </li>
                              <li>
                                一个用于添加或更新预报卡片的方法 (<code>app.updateForecastCard</code>)。
                              </li>
                              <li>
                                一个用于从 Firebase Public Weather API 获取最新天气预报数据的方法 (<code>app.getForecast</code>)。
                              </li>
                            </ul>
                            
                            <h2>
                              使用服务工作线程缓存预报数据
                            </h2>
                            
                            <p>
                              为您的数据选择正确的<a href="https://jakearchibald.com/2014/offline-cookbook/">缓存策略</a>至关重要，这取决于您的应用所提供数据的类型。例如，天气或股票价格之类的高时效性数据应尽可能频繁地更新，而头像图像或文章内容的更新则不必那么频繁。
                            </p>
                            
                            <p>
                              <a href="https://jakearchibald.com/2014/offline-cookbook/#cache-network-race">缓存优先于网络</a> 策略是适合我们的应用的理想策略。它会尽快将数据显示在屏幕上，然后在网络返回最新数据时更新数据。与网络优先于缓存相比，用户不必等到<a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">抓取</a>超时，就能获得缓存的数据。
                            </p>
                            
                            <p>
                              缓存优先于网络意味着我们需要发起两个异步请求，一个发向缓存，一个发向网络。我们通过应用发出的网络请求不需要做多大的改动，但我们需要修改服务工作线程，以先缓存响应，然后再将其返回。
                            </p>
                            
                            <p>
                              在正常情况下，将返回缓存的数据，这几乎立即能够为应用提供其可以使用的最新数据。然后，在网络请求返回时，将使用来自网络的最新数据更新应用。
                            </p>
                            
                            <h3>
                              拦截网络请求并缓存响应
                            </h3>
                            
                            <p>
                              我们需要修改服务工作线程，以拦截发给 weather API 的请求，并将其响应存储在缓存内，这样我们就能在稍后方便地访问它们。采用缓存优先于网络策略时，我们期望网络响应成为“可信来源”，始终能够为我们提供最新信息。如果它无法做到，失败了也无关紧要，因为我们已经检索了应用内最新的缓存数据。
                            </p>
                            
                            <p>
                              让我们在服务工作线程中添加一个 <code>dataCacheName</code>，以便将应用数据与 App Shell 分离。更新 App Shell 并清除较旧缓存时，我们的数据将保持不变，可随时用于实现超快速加载。切记，如果未来您的数据格式发生变化，您需要相应的处理手段，并且需要确保 App Shell 和内容保持同步。
                            </p>
                            
                            <p>
                              向您的 <code>service-worker.js</code> 文件顶部添加下面一行：
                            </p>
                            
                            <pre><code>var dataCacheName = 'weatherData-v1';
</code></pre>
                            
                            <p>
                              接下来，更新 <code>activate</code> 事件处理程序，使其不会在清除 App Shell 缓存时删除数据缓存。
                            </p>
                            
                            <pre><code>if (key !== cacheName && key !== dataCacheName) {
</code></pre>
                            
                            <p>
                              最后，更新 <code>fetch</code> 事件处理程序，将发给 data API 的请求与其他请求分开处理。
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(e) {
  console.log('[Service Worker] Fetch', e.request.url);
  var dataUrl = 'https://query.yahooapis.com/v1/public/yql';
  if (e.request.url.indexOf(dataUrl) &gt; -1) {
    /*
     * When the request URL contains dataUrl, the app is asking for fresh
     * weather data. In this case, the service worker always goes to the
     * network and then caches the response. This is called the "Cache then
     * network" strategy:
     * https://jakearchibald.com/2014/offline-cookbook/#cache-then-network
     */
    e.respondWith(
      caches.open(dataCacheName).then(function(cache) {
        return fetch(e.request).then(function(response){
          cache.put(e.request.url, response.clone());
          return response;
        });
      })
    );
  } else {
    /*
     * The app is asking for app shell files. In this scenario the app uses the
     * "Cache, falling back to the network" offline strategy:
     * https://jakearchibald.com/2014/offline-cookbook/#cache-falling-back-to-network
     */
    e.respondWith(
      caches.match(e.request).then(function(response) {
        return response || fetch(e.request);
      })
    );
  }
});
</code></pre>
                            
                            <p>
                              以上代码会拦截请求，并检查网址是否以 weather API 的地址开头。如果是，我们将使用<a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">抓取</a>发出请求。返回请求后，我们的代码会打开缓存，克隆响应，将其存储在缓存内，最后将响应返回给原始请求者。
                            </p>
                            
                            <p>
                              我们的应用尚不能完全离线工作。我们已经实现了 App Shell 的缓存和检索，但尽管我们缓存了数据，应用也没有查看缓存来看看其中是否有任何天气数据。
                            </p>
                            
                            <h3>
                              发出请求
                            </h3>
                            
                            <p>
                              如前文所述，应用需要发起两个异步请求，一个发向缓存，一个发向网络。应用利用 <code>window</code> 中提供的 <code>caches</code> 对象来访问缓存和检索最新数据。这是“渐进式增强”的一个典范，因为可能并非所有浏览器都提供 <code>caches</code> 对象，即使没有该对象，网络请求仍应正常工作。
                            </p>
                            
                            <p>
                              为此，我们需要做的是：
                            </p>
                            
                            <ol start="1">
                              <li>
                                检查全局 <code>window</code> 对象中是否提供了 <code>caches</code> 对象。
                              </li>
                              
                              <li>
                                从缓存请求数据。
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                首次运行时，您的应用应该会立即向用户显示来自 <code>initialWeatherForecast</code> 的预报。
                              </li>
                            </ul>
                            
                            <ol start="3">
                              <li>
                                从服务器请求数据。
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                服务工作线程取消注册后，它可能仍存在于列表内，直至包含它的浏览器窗口关闭为止。
                              </li>
                              <li>
                                如果您的应用有多个窗口处于打开状态，则新服务工作线程在这些窗口都已重新加载并更新到最新服务工作线程后才会生效。
                              </li>
                            </ul>
                            
                            <h4>
                              从缓存获取数据
                            </h4>
                            
                            <p>
                              接下来，我们需要检查并确保 <code>caches</code> 对象存在，并从该对象请求最新数据。找到 <code>app.getForecast()</code> 中的 <code>TODO add cache logic here</code> 注释，然后在注释下方添加以下代码。
                            </p>
                            
                            <pre><code>    if ('caches' in window) {
      /*
       * Check if the service worker has already cached this city's weather
       * data. If the service worker has the data, then display the cached
       * data while the app fetches the latest data.
       */
      caches.match(url).then(function(response) {
        if (response) {
          response.json().then(function updateFromCache(json) {
            var results = json.query.results;
            results.key = key;
            results.label = label;
            results.created = json.query.created;
            app.updateForecastCard(results);
          });
        }
      });
    }
</code></pre>
                            
                            <p>
                              我们的天气应用现在会发出两个异步数据请求，一个从 <code>cache</code> 发出，一个通过 XHR 发出。如果缓存中有数据，将会返回响应并以极快的速度（几十微秒）渲染，并在 XHR 仍未完成时更新卡片。然后，在 XHR 响应时，将直接使用 weather API 的最新数据更新卡片。
                            </p>
                            
                            <p>
                              请注意缓存请求和 XHR 请求都以更新预报卡片的调用结束。应用如何知道它是否显示了最新数据？<code>app.updateForecastCard</code> 中的以下代码会处理这个问题：
                            </p>
                            
                            <pre><code>    var cardLastUpdatedElem = card.querySelector('.card-last-updated');
    var cardLastUpdated = cardLastUpdatedElem.textContent;
    if (cardLastUpdated) {
      cardLastUpdated = new Date(cardLastUpdated);
      // Bail if the card has more recent data then the data
      if (dataLastUpdated.getTime() &lt; cardLastUpdated.getTime()) {
        return;
      }
    }
</code></pre>
                            
                            <p>
                              每次更新卡片时，应用都会在卡片上隐藏的属性中存储数据的时间戳。仅当卡片上已经存在的时间戳晚于传递给函数的数据时，应用才会释放。
                            </p>
                            
                            <h3>
                              测试
                            </h3>
                            
                            <p>
                              现在应用应能完全离线运行了。保存两个城市，按应用上的 refresh 按钮获取最新的天气数据，然后转为离线状态并重新加载页面。
                            </p>
                            
                            <p>
                              转至 DevTools 的 <strong>Application</strong> 面板的 <strong>Cache Storage</strong> 窗格。展开这个部分，您会看到左侧列出的 App Shell 和数据缓存的名称。如果每个城市存储有数据，即可打开数据缓存。
                            </p>
                            
                            <p>
                              <img src="img/cf095c2153306fa7.png" alt="cf095c2153306fa7.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-07/">链接</a>
                            </p>
                            
                            <h2>
                              支持本机集成
                            </h2>
                            
                            <p>
                              没人愿意多此一举地在移动设备键盘上输入长长的网址。通过 Add To Home Screen 功能，您的用户可以像从应用商店安装本机应用那样，选择为其设备添加一个快捷链接，并且过程要顺畅得多。
                            </p>
                            
                            <h3>
                              适用于 Android 版 Chrome 的网络应用安装横幅和 Add To Home Screen
                            </h3>
                            
                            <p>
                              网络应用安装横幅让您的用户可以快速无缝地将您的网络应用添加到他们的主屏幕，简化了启动和返回应用的操作。添加应用安装横幅很轻松，Chrome 为您处理大部分的繁重工作。我们只需加入一个包含应用详细信息的网络应用清单文件。
                            </p>
                            
                            <p>
                              然后，Chrome 使用一组条件（包括使用服务工作线程、SSL 状态）和访问频率启发式算法来确定何时显示横幅。此外，用户还可通过 Chrome 中的“Add to Home Screen”菜单按钮手动添加它。
                            </p>
                            
                            <h4>
                              通过 <code>manifest.json</code> 文件声明应用清单
                            </h4>
                            
                            <p>
                              网络应用清单是一个简单的 JSON 文件，使您（开发者）能够控制在用户可能看到应用的区域（例如手机主屏幕）中如何向用户显示应用，指示用户可以启动哪些功能，更重要的是说明启动方法。
                            </p>
                            
                            <p>
                              利用网络应用清单，您的网络应用可以：
                            </p>
                            
                            <ul>
                              <li>
                                如果服务器请求仍未完成，则用缓存的数据更新应用。
                              </li>
                              <li>
                                Be launched in full-screen mode on Android with no URL bar
                              </li>
                              <li>
                                Control the screen orientation for optimal viewing
                              </li>
                              <li>
                                Define a "splash screen" launch experience and theme color for the site
                              </li>
                              <li>
                                Track whether you're launched from the home screen or URL bar
                              </li>
                            </ul>
                            
                            <p>
                              在您的 <code>work</code> 文件夹中创建名称为 <code>manifest.json</code> 的文件，并复制/粘贴以下内容：
                            </p>
                            
                            <pre><code>{
  "name": "Weather",
  "short_name": "Weather",
  "icons": [{
    "src": "images/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    }],
  "start_url": "/index.html",
  "display": "standalone",
  "background_color": "#3E4EB8",
  "theme_color": "#2F3BA2"
}
</code></pre>
                            
                            <p>
                              此清单支持一组适用于各种屏幕尺寸的图标。在编写此内容时，Chrome 和 Opera Mobile（支持网络应用清单的两个浏览器）不会使用像素小于 192px 的任何内容。
                            </p>
                            
                            <p>
                              跟踪应用如何启动的一个简单方式就是向 <code>start_url</code> 参数添加查询字符串，然后使用 Analytics Suite 跟踪查询字符串。如果您使用此方法，请记得更新 App Shell 缓存的文件列表，以确保缓存了带有查询字符串的文件。
                            </p>
                            
                            <h4>
                              将清单文件的相关信息告知浏览器
                            </h4>
                            
                            <p>
                              现在将以下行添加到 <code>index.html</code> 文件中的 <code>&lt;head&gt;</code> 元素底部：
                            </p>
                            
                            <pre><code>&lt;link rel="manifest" href="/manifest.json"&gt;
</code></pre>
                            
                            <h4>
                              最佳做法
                            </h4>
                            
                            <ul>
                              <li>
                                保存数据以便稍后快速访问。
                              </li>
                              <li>
                                使用来自服务器的最新数据更新应用。
                              </li>
                              <li>
                                Define icon sets for different density screens. Chrome will attempt to use the icon closest to 48dp, for example, 96px on a 2x device or 144px for a 3x device.
                              </li>
                              <li>
                                Remember to include an icon with a size that is sensible for a splash screen and don't forget to set the <code>background_color</code>.
                              </li>
                            </ul>
                            
                            <p>
                              深入阅读：
                            </p>
                            
                            <p>
                              <a href="/web/fundamentals/engage-and-retain/simplified-app-installs/">使用应用安装横幅</a>
                            </p>
                            
                            <h3>
                              适用于 iOS 版 Safari 的 Add to Home Screen 元素
                            </h3>
                            
                            <p>
                              在您的 <code>index.html</code> 中，向 <code>&lt;head&gt;</code> 元素底部添加以下代码：
                            </p>
                            
                            <pre><code>  &lt;!-- Add to home screen for Safari on iOS --&gt;
  &lt;meta name="apple-mobile-web-app-capable" content="yes"&gt;
  &lt;meta name="apple-mobile-web-app-status-bar-style" content="black"&gt;
  &lt;meta name="apple-mobile-web-app-title" content="Weather PWA"&gt;
  &lt;link rel="apple-touch-icon" href="images/icons/icon-152x152.png"&gt;
</code></pre>
                            
                            <h3>
                              适用于 Windows 平台的平铺图标
                            </h3>
                            
                            <p>
                              在您的 <code>index.html</code> 中，向 <code>&lt;head&gt;</code> 元素底部添加以下代码：
                            </p>
                            
                            <pre><code>  &lt;meta name="msapplication-TileImage" content="images/icons/icon-144x144.png"&gt;
  &lt;meta name="msapplication-TileColor" content="#2F3BA2"&gt;
</code></pre>
                            
                            <h3>
                              测试
                            </h3>
                            
                            <p>
                              在此部分中，我们将向您介绍测试网络应用清单的两种方式。
                            </p>
                            
                            <p>
                              第一种方式是 DevTools。打开 __Application __面板上的 __Manifest __窗格。如果您已正确添加清单信息，将会看到系统已将其解析并以人性化的形式显示在此窗格中。
                            </p>
                            
                            <p>
                              您还可以从此窗格中测试添加到主屏幕功能。点击 __Add to homescreen __按钮。您会在网址栏下方看到“add this site to your shelf”消息，如以下屏幕所示。
                            </p>
                            
                            <p>
                              <img src="img/cbfdd0302b611ab0.png" alt="cbfdd0302b611ab0.png" />
                            </p>
                            
                            <p>
                              移动应用的添加到主屏幕功能与桌面应用一样。如果您能在桌面应用中成功触发此提示，就肯定会知道移动用户可以将您的应用添加到他们的设备中。
                            </p>
                            
                            <p>
                              第二种测试方式通过 Web Server for Chrome 实现。通过这种方法，您向其他计算机提供了自己的本地开发服务器（在台式机或笔记本电脑上），然后您只能从真实的移动设备中访问自己的 Progressive Web App。
                            </p><aside class="warning">

<p>Opening up a port for remote access is handy for testing this step but may be blocked by your computer's firewall rules or network administrator. Opening ports for remote access is generally not a good thing to leave running on your computer. So, for security reasons, when you've completed testing this step, disable the <code>Accessible on local network</code> option and restart your web server.</p>

</aside> 
                            
                            <p>
                              在 Web Server for Chrome 配置对话框中，选择 <code>Accessible on local network</code> 选项：
                            </p>
                            
                            <p>
                              <img src="img/81347b12f83e4291.png" alt="81347b12f83e4291.png" />
                            </p>
                            
                            <p>
                              将网络服务器切换到 <code>STOPPED</code>，然后重新切换到 <code>STARTED</code>。您将会看到新的网址，可用于远程访问您的应用。
                            </p>
                            
                            <p>
                              现在，使用新网址从移动设备访问您的网站。
                            </p>
                            
                            <p>
                              如果使用这种测试方式，您会在控制台中看到服务工作线程错误，这是因为未通过 HTTPS 提供服务工作线程。
                            </p>
                            
                            <p>
                              在移动 Android 设备上使用 Chrome，尝试将应用添加到主屏幕并验证启动屏幕能否正确显示以及使用的图标是否正确。
                            </p>
                            
                            <p>
                              在 Safari 和 Internet Explorer 中，您还可以手动将应用添加到主屏幕。
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-08/">链接</a>
                            </p>
                            
                            <h2>
                              部署到一个安全主机，然后大功告成
                            </h2>
                            
                            <p>
                              最后一步是将我们的天气应用部署到一个支持 HTTPS 的服务器。如果您还没有此类服务器，可以采用一个绝对容易（并且免费）的方法，那就是使用 Firebase 托管的静态内容。它使用起来极其简便，通过 HTTPS 提供内容，并由全球 CDN 提供支持。
                            </p>
                            
                            <h3>
                              额外好处：缩小和内联 CSS
                            </h3>
                            
                            <p>
                              还有一点是您应该考虑的，那就是缩小关键样式并将它们直接内联到 <code>index.html</code> 中。<a href="/speed">PageSpeed Insights</a> 建议在请求的前 15k 字节中提供首屏内容。
                            </p>
                            
                            <p>
                              看一看在将所有内容内联的情况下可以将首次请求缩小多少。
                            </p>
                            
                            <p>
                              深入阅读：<a href="/speed/docs/insights/rules">PageSpeed Insights 规则</a>
                            </p><aside class="key-point">

<p>This step requires you to have  <a href="https://docs.npmjs.com/getting-started/installing-node">Node &#x26; NPM</a> installed on your system. If it's not, you can use any other hosting provider that supports HTTP<strong>S</strong>. We've used Firebase because it automatically redirects users from HTTP to HTTP<strong>S</strong>. If you use a different provider, be sure they're always redirects to HTTP<strong>S</strong>.</p>

</aside> 
                            
                            <h3>
                              部署至 Firebase
                            </h3>
                            
                            <p>
                              如果您从未接触过 Firebase，需要先创建您的帐户并安装一些工具。
                            </p>
                            
                            <ol start="1">
                              <li>
                                在 <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a> 上创建一个 Firebase 帐户
                              </li>
                              
                              <li>
                                通过 npm 安装 Firebase：<code>npm install -g firebase-tools</code>
                              </li>
                            </ol>
                            
                            <p>
                              创建帐户并登录后，便可随时进行部署！
                            </p>
                            
                            <ol start="1">
                              <li>
                                在 <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a> 上创建一个新应用
                              </li>
                              
                              <li>
                                如果您近期未登录 Firebase 工具，请更新您的凭据：<code>firebase login</code>
                              </li>
                              
                              <li>
                                初始化您的应用，并提供您完成的应用所在的目录（很可能是 <code>work</code>）： <code>firebase init</code>
                              </li>
                              
                              <li>
                                最后，将应用部署到 Firebase： <code>firebase deploy</code>
                              </li>
                              
                              <li>
                                大功告成。 操作完成！您的应用将部署到以下网域：<code>https://YOUR-FIREBASE-APP.firebaseapp.com</code>
                              </li>
                            </ol>
                            
                            <p>
                              深入阅读：<a href="https://www.firebase.com/docs/hosting/guide/">Firebase 托管指南</a>
                            </p>
                            
                            <h3>
                              测试
                            </h3>
                            
                            <ul>
                              <li>
                                在用户的 Android 主屏幕进行丰富的呈现
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/final/">链接</a>
                            </p>
                            
                            <h2>
                              发现问题，或者有反馈？{: .hide-from-toc }
                            </h2>
                            
                            <p>
                              立即提交<a href="https://github.com/googlecodelabs/your-first-pwapp/issues">问题</a>，帮助我们让代码实验室更加强大。 谢谢！
                            </p>