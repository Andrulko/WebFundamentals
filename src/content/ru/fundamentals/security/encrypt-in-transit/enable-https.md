project_path: /web/fundamentals/_project.yaml book_path: /web/fundamentals/_book.yaml description: Enabling HTTPS on your servers is critical to securing your webpages.

{# wf_updated_on: 2018-02-12 #} {# wf_published_on: 2015-03-27 #} {# wf_blink_components: Blink>SecurityFeature,Internals>Network>SSL #}

# Enabling HTTPS on Your Servers {: .page-title }

{% include "web/_shared/contributors/chrispalmer.html" %} {% include "web/_shared/contributors/mattgaunt.html" %}

### TL;DR {: .hide-from-toc }

* Create a 2048-bit RSA public/private key pair.
* Generate a certificate signing request (CSR) that embeds your public key.
* Share your CSR with your Certificate Authority (CA) to receive a final certificate or a certificate chain.
* Install your final certificate in a non-web-accessible place such as `/etc/ssl` (Linux and Unix) or wherever IIS requires it (Windows).

## Generating keys and certificate signing requests

В данном разделе для генерации открытых/закрытых ключей и запросов на получение сертификата CSR используется консольная программа openssl, стандартная для большинства операционных систем Linux, BSD и Mac OS X.

### TL;DR {: .hide-from-toc }

В данном примере мы создадим пару 2048-разрядных ключей с использованием криптосистемы RSA. (Более короткие ключи, например 1024-разрядные ключи, могут быть подобраны с использованием метода полного перебора возможных комбинаций. Более длинные ключи , например 4096-разрядные,&nbsp;использовать нецелесообразно. С течением времени длина ключей растет, поскольку вычислительные мощности дешевеют. Идеальным вариантом на сегодняшний день являются 2048-разрядные ключи.

Ниже приведена команда для генерации пары ключей с использованием криптосистемы RSA:

    openssl genrsa -out www.example.com.key 2048
    

Она возвращает следующее:

    Generating RSA private key, 2048 bit long modulus
    .+++
    .......................................................................................+++
    e is 65537 (0x10001)
    

### Производительность

На этом этапе вы размещаете свой открытый ключ и информацию о своей организации и вашем веб-сайте в запросе на получение сертификата, и эти данные запросит у вас *openssl*.

Выполнение этой команды:

    openssl req -new -sha256 -key www.example.com.key -out
    

www.example.com.csr

    You are about to be asked to enter information that will be incorporated
    into your certificate request
    
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:CA
    State or Province Name (full name) [Some-State]:California
    Locality Name (eg, city) []:Mountain View
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:Example, Inc.
    Organizational Unit Name (eg, section) []:Webmaster Help Center Example
    Team
    Common Name (e.g. server FQDN or YOUR name) []:www.example.com
    Email Address []:webmaster@example.com
    
    Please enter the following 'extra' attributes
    to be sent with your certificate request
    A challenge password []:
    An optional company name []:
    

Вернет следующее:

    openssl req -text -in www.example.com.csr -noout
    

Теперь необходимо убедиться в корректности CSR; это можно сделать с помощью следующей команды:

    Certificate Request:
        Data:
            Version: 0 (0x0)
            Subject: C=CA, ST=California, L=Mountain View, O=Google, Inc.,
    OU=Webmaster Help Center Example Team,
    CN=www.example.com/emailAddress=webmaster@example.com
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (2048 bit)
                    Modulus:
                        00:ad:fc:58:e0:da:f2:0b:73:51:93:29:a5:d3:9e:
                        f8:f1:14:13:64:cc:e0:bc:be:26:5d:04:e1:58:dc:
                        ...
                    Exponent: 65537 (0x10001)
            Attributes:
                a0:00
        Signature Algorithm: sha256WithRSAEncryption
             5f:05:f3:71:d5:f7:b7:b6:dc:17:cc:88:03:b8:87:29:f6:87:
             2f:7f:00:49:08:0a:20:41:0b:70:03:04:7d:94:af:69:3d:f4:
             ...
    

### Заголовки источников ссылок

Ответ программы должен выглядеть следующим образом:

В зависимости от сертифицирующего органа возможно использование различных способов подачи запроса на получение сертификата: через форму на веб-сайте органа, по электронной почте или иным способом. Некоторые сертифицирующие органы (либо их торговые посредники) могут автоматически выполнять некоторые или все из вышеуказанных шагов (которые порой могут включать в себя генерацию пары ключей и CSR).

Отправьте сертифицирующему органу свой CSR и следуйте предоставляемым инструкциям для получения окончательного сертификата или цепочки сертификатов.

Различные сертифицирующие органы взимают различную плату за услуги сертификации вашего открытого ключа.

Также существует возможность назначить ваш ключ нескольким доменным именам, включая несколько конкретных имен (например, example.com, www.example.com, example.net, и www.example.net), а также группе имен с подстановочными символами, например, \*.example.com.

* Для этого вам понадобится создать пару из открытого и закрытого 2048-разрядных ключей с использованием криптосистемы RSA.
* Сформируйте запрос на получение сертификата (CSR), в который встраивается ваш открытый ключ.

Например, один из сертифицирующих органов предлагает следующие расценки:

По этим расценкам получение сертификатов для группы доменных имен становится выгодным, если у вас в наличии более 9 дочерних доменов; в остальных случаях вы можете приобрести сертификаты на каждое имя по отдельности. Если , например, у вас более 5 дочерних доменов, получение сертификатов для группы имен может оказаться для вас более удобным, если вам понадобится использовать протокол HTTPS на своих серверах).

**ПРИМЕЧАНИЕ.** Помните, что групповой пакет сертификатов предполагает подстановку символов только в первой части доменного имени. Сертификат для \*.example.com подойдет для foo.example.com и bar.example.com, но не для foo.bar.example.com.

## Создание пары открытого/закрытого ключей

Поместите сертификат в хранилища на внешних серверах, куда невозможно попасть через Интернет , например в /etc/ssl (для Linux и Unix), или в другое место, рекомендуемое IIS (для Windows).

* Стандартный пакет: $16/год для доменных имен example.com и www.example.com.
* Групповой пакет: $150/год для доменных имен example.com и \*.example.com.

На этом этапе необходимо принять важнейшее эксплуатационное решение:

* выделять отдельный IP-адрес для каждого имени хоста, с которого получает контент ваш веб-сервер, или
* использовать виртуальный хостинг на базе имен.

Если для каждого имени хоста используется отдельный IP-адрес, это замечательно! Вы можете с легкостью поддерживать HTTP и HTTPS для всех клиентов.

Однако операторы большинства сайтов используют виртуальный хостинг на базе имен — это позволяет сохранить IP-адреса и в общем и целом более удобно. Проблема с IE в Windows XP и Android версии ранее 2.3 состоит в том, что они не понимают обозначение имени сервера [Server Name Indication](https://en.wikipedia.org/wiki/Server_Name_Indication) (SNI), которое является ключевым для виртуального хостинга на базе имен для HTTPS.

Когда-нибудь, будем надеяться, что скоро, на смену клиентам, которые не поддерживают SNI, придет более современное программное обеспечение. Контролируйте строку агента пользователя в журналах запросов, чтобы узнать, когда достаточное количество ваших пользователей перейдет на современное программное обеспечение. (Вы можете решить, что ваш порог составляет, скажем &lt; 5 % или &lt; 1 % и т. п.)

Если на ваших серверах еще нет службы HTTPS, включите ее сейчас (без перенаправления HTTP на HTTPS, см. ниже). Настройте свой веб-сервер на использование приобретенных и установленных сертификатов. Возможно, [удобный генератор конфигурации Mozilla](https://mozilla.github.io/server-side-tls/ssl-config-generator/) покажется вам полезным.

Если у вас много имен хостов/дочерних доменов, каждый из них должен использовать корректный сертификат.

Note: Операторы многих сайтов уже выполнили указанные нами шаги, но продолжают использовать HTTPS с единственной целью перенаправлять клиентов обратно на HTTP. Если вы так поступаете, прекратите немедленно. В следующем разделе показано, как обеспечить бесперебойную работу HTTPS и HTTP.

Warning: В конце концов вы должны перенаправлять запросы HTTP на HTTPS и использовать строгую безопасность транспорта HTTP Strict Transport Security (HSTS). Делать это на этапе процесса миграции не следует. См. разделы "Перенаправление с HTTP на HTTPS" и "Включение строгой безопасности транспорта и использование защищенных файлов cookie".

Теперь, пока существует ваш сайт, проверяйте конфигурацию HTTPS с помощью [удобного теста SSL Server Test Qualys](https://www.ssllabs.com/ssltest/){: .external }. Ваш сайт должен получать оценку A или A+. Любые причины более низкой оценки должны расцениваться как ошибка. (Сегодняшний уровень A завтра превратится в B, поскольку атаки на алгоритмы и протоколы становятся все более изощренными!)

## Генерация запроса на получение сертификата CSR

Проблема возникает, когда вы сталкиваетесь со страницей протокола HTTPS , содержащую HTTP-ресурсы: [смешанный контент](http://www.w3.org/TR/mixed-content/), при этом браузеры предупреждают пользователя о том, что протокол HTTPS работает не в полную силу.

На самом деле в случае активного смешанного контента (скрипты, плагины, таблицы стилей CSS, плавающие фреймы iframe), браузеры зачастую просто не загружают и не исполняют контент — в результате мы получаем нерабочую страницу.

**ПРИМЕЧАНИЕ.** Все работает, когда на HTTP-странице включены HTTPS-ресурсы.

Кроме того, когда на своем сайте вы ссылаетесь на другие страницы, пользователи могут перейти с протокола HTTPS на HTTP.

Такие проблемы возникают, когда на вашей странице есть полноценные внутренние ссылки, использующие схему *http://*. Вам следует изменить контент следующим образом:

замените на:

или:

<pre class="prettyprint">&lt;h1>Welcome To Example.com&lt;/h1>
&lt;script src="<b>http://</b>example.com/jquery.js">&lt;/script>
&lt;link rel="stylesheet" href="<b>http://</b>assets.example.com/style.css"/>
&lt;img src="<b>http://</b>img.example.com/logo.png"/>;
&lt;p>A &lt;a href="<b>http://</b>example.com/2014/12/24/">new post on cats!&lt;/a>&lt;/p>
</pre>

Таким образом внутренние ссылки становятся как можно более относительными: либо относятся к протоколу (не имеют протокола, начинаются с //example.com), либо к хосту (начинаются с пути, например, с /jquery.js).

**ПРИМЕЧАНИЕ.** Делайте это не вручную, а с помощью скрипта. Если содержимое вашего сайта находится в базе данных, протестируйте ваш скрипт на резервной версии базы данных, используемой для разработки. Если содержимое вашего сайта – простые файлы, протестируйте ваш скрипт на резервной копии файлов, используемой для разработки. Отправляйте изменения в производство только после того, как они прошли проверку на качество. Можете использовать скрипт \[Bram van Damme's\] (https://github.com/bramus/mixed-content-scan) или аналогичные решения, чтобы определить смешанный контент на вашем сайте.

<pre class="prettyprint">&lt;h1>Welcome To Example.com&lt;/h1>
&lt;script src="<b>/jquery.js</b>">&lt;/script>
&lt;link href="<b>/styles/style.css</b>" rel="stylesheet"/>
&lt;img src="<b>/images/logo.png</b>"/>;
&lt;p>A &lt;a href="<b>/2014/12/24/</b>">new post on cats!&lt;/a>&lt;/p>
</pre>

**ПРИМЕЧАНИЕ.** При указании ссылок на другие сайты (вместо того чтобы включать фрагменты их ресурсов) не изменяйте протокол, поскольку вы не контролируете работу этих сайтов.

<pre class="prettyprint">&lt;h1>Welcome To Example.com&lt;/h1>
&lt;script src="<b>//example.com/jquery.js</b>">&lt;/script>
&lt;link href="<b>//assets.example.com/style.css</b>" rel="stylesheet"/>
&lt;img src="<b>//img.example.com/logo.png</b>"/>;
&lt;p>A &lt;a href="<b>//example.com/2014/12/24/</b>">new post on cats!&lt;/a>&lt;/p>
</pre>

**ПРИМЕЧАНИЕ.** Я рекомендую ссылки, зависящие от протокола, с тем чтобы миграция больших сайтов проходила более гладко. Если вы не уверены, что сможете полностью перейти на HTTPS, принудительное использование HTTPS для всех ресурсов вашего сайта может стать рискованным. Скорее всего, поначалу HTTPS будет казаться вам новым и непонятным, и в этот период HTTP-версия должна продолжать нормально работать. Спустя некоторое время вы завершите миграцию и можете работать только в HTTPS (см. следующие два раздела).

<pre class="prettyprint">&lt;h1>Welcome To Example.com&lt;/h1>
&lt;script src="/jquery.js">&lt;/script>
&lt;link href="/styles/style.css" rel="stylesheet"/>
&lt;img src="/images/logo.png"/>;
&lt;p>A &lt;a href="/2014/12/24/">new post on cats!&lt;/a>&lt;/p>
&lt;p>Check out this &lt;a href="<b>https://foo.com/</b>">other cool site.&lt;/a>&lt;/p>
</pre>

Если ваш сайт зависит от скрипта, изображения или других ресурсов третьих сторон, например от CDN, jquery.com и т. д., у вас есть два варианта:

*Использовать для этих ресурсов ссылки, также зависящие от протокола. Если третья сторона не работает с HTTPS, попросите их об этом. В большинстве своем такие третьи лица уже поддерживают этот протокол, например jquery.com.

Помните, что вам также будет необходимо изменить URL-адреса внутри сайта в таблицах стилей, JavaScript, правилах перенаправления, &lt;ссылочных...&gt; тегах и синтаксических конструкциях на CSP-страницах, а не только на HTML-страницах!

Вставьте на свои страницы теги `<link rel="canonical" href="https://…"/>`. [Это позволит поисковым системам](https://support.google.com/webmasters/answer/139066) определить наилучший способ перейти на ваш сайт.

* Используйте ресурсы со своих серверов, работающих как с протоколом HTTP, так и с HTTPS. Это всегда целесообразно, поскольку дает вам возможность лучше контролировать внешний вид, производительность и безопасность вашего сайта – вам не нужно полагаться на третьих лиц, что всегда неплохо.
* Serve the resources from a server that you control, and which offers both HTTP and HTTPS. This is often a good idea anyway, because then you have better control over your site's appearance, performance, and security. In addition, you don't have to trust a third party, which is always nice.

Большинство веб-серверов оснащены простой функцией перенаправления. Воспользуйтесь кодом состояния 301 (окончательно перемещено), чтобы указать поисковым системам и браузерам, что версия HTTPS является канонической, и перенаправить пользователей на версию HTTPS вашего сайта с версии HTTP.

## Предоставление CSR сертифицирующему органу

На этом этапе мы уже "зафиксировали" использование протокола HTTPS. Сначала воспользуйтесь [механизмом Strict Transport Security](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security), чтобы указать клиентам, что для подключения к вашему серверу следует всегда использовать протокол HTTPS, даже при переходе по ссылке http://. Это позволит защититься от атак типа [SSL Strip](http://www.thoughtcrime.org/software/sslstrip/){: .external }, а также избежать затрат, связанных с кодом перенаправления 301, о чем мы рассказали в разделе "Перенаправление с HTTP на HTTPS".

**ПРИМЕЧАНИЕ.** Существует высокая вероятность, что у клиентов, которые отметили ваш сайт как известный узел HSTS, возникнет серьезный сбой](https://tools.ietf.org/html/rfc6797#section-12.1), _[если в конфигурации TLS ](https://tools.ietf.org/html/rfc6797#section-12.1)[ вашего сайта имеется ошибка ](https://tools.ietf.org/html/rfc6797#section-12.1) (например, просроченный сертификат). Такое поведение явным образом задано в самом механизме HSTS; это позволяет гарантировать, что злоумышленники не смогут обмануть клиентов, заставив их перейти на сайт по незащищенному протоколу. Не включайте HSTS, пока тщательно не проработаете ваш сайт, чтобы избежать развертывания HTTPS с ошибками проверки сертификата.

## Enable HTTPS on your servers

Чтобы включить механизм HSTS, задайте заголовок Strict-Transport-Security. [На странице OWASP, посвященной HSTS, имеются ссылки на соответствующие инструкции](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security) для различного серверного ПО.

* Другим сайтам следует перейти на HTTPS. В таком случае владельцам сайтов будет полезно ознакомиться с этим руководством! :) Если владелец сайта, на который имеется ссылка, выполнит инструкции из раздела данного руководства "Включение HTTPS на ваших серверах", вы можете изменить на своем сайте формат ссылок на другие сайты (поменять http:// на https://) или воспользоваться ссылками на основе протокола.
* Можно воспользоваться новым [стандартом политики в отношении источников ссылок](http://www.w3.org/TR/referrer-policy/#referrer-policy-delivery-meta) , чтобы обойти различные проблемы, связанные с заголовками источников ссылок.

Большинство веб-серверов оснащены аналогичной функцией добавления настраиваемых заголовков.

**ПРИМЕЧАНИЕ.** Значение параметра "max-age" задается в секундах. Для начала можно указать небольшие значения, а затем постепенно увеличивать значение параметра "max-age" по мере более уверенного перехода на использование только HTTPS для своего сайта.

Также важно обеспечить, чтобы клиенты никогда не отправляли файлы cookie (например, для аутентификации или передачи настроек сайта) по протоколу HTTP. Например, если файл cookie пользователя с данными аутентификации представлен в виде обычного текста, безопасность всего сеанса подключения гарантированно будет нарушена, даже если все остальное было сделано правильно.

Поэтому ваше приложение должно всегда устанавливать флаг Secure для файлов cookie. [На этой странице OWASP поясняется, каким образом флаг Secure](https://www.owasp.org/index.php/SecureFlag) устанавливается в ряде платформ приложений. В каждой платформе приложений имеется способ установить такой флаг.

[Google рассматривает использование HTTPS как один из показателей положительного качества сайта при указании его в результатах поиска](https://googlewebmastercentral.blogspot.com/2014/08/https-as-ranking-signal.html). Также Google предлагает ознакомиться с руководством [по переносу, перемещению или миграции сайта](https://support.google.com/webmasters/topic/6029673) с сохранением его поискового рейтинга. [Аналогичные инструкции для веб-мастеров](http://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) предлагает и Bing.

Когда работа над слоями контента и приложений завершена (прекрасные рекомендации по этому вопросу изложены в [руководствах Стива Саудерса](https://stevesouders.com/){: .external }), остается такая мелочь по сравнению с общими затратами на приложение , как настройка производительности TLS. Кроме того, у вас есть возможность сократить эти затраты и даже полностью избавиться от них. (Отличные советы по поводу оптимизации TLS и не только предлагаются в книге *[High Performance Browser Networking](https://hpbn.co/)_[ Ильи Григорика](https://hpbn.co/).) Также рекомендуем ознакомиться с книгами _[OpenSSL Cookbook](https://www.feistyduck.com/books/openssl-cookbook/)* и *[Bulletproof SSL And TLS](https://www.feistyduck.com/books/bulletproof-ssl-and-tls/)* Айвана Ристика.

В некоторых случаях *повысить* производительность TLS можно в основном за счет реализации возможностей HTTP/2. Об этом говорил Крис Палмер [в своем выступлении, посвященном производительности HTTPS и HTTP/2, на саммите разработчиков Chrome 2014](/web/shows/cds/2014/tls-all-the-things).

При переходе пользователей по ссылкам на другие сайты HTTP, которые имеются на вашем сайте HTTPS, агенты пользователей не передают заголовок источника ссылки. Если это нежелательно, существует несколько способов исправить ситуацию.

## Make intrasite URLs relative

Поскольку поисковые системы переходят на HTTPS, не исключено, что к моменту, когда вы перейдете на HTTPS, заголовков источников ссылок будет *больше*, чем сейчас.

### Доход от рекламы

Операторы сайтов, получающие доход от рекламы, размещенной на своих сайтах, хотят убедиться, что переход на HTTPS не скажется негативно на показах рекламы. Однако ввиду требований к безопасности смешанного контента элементы iframe HTTP блокируются на странице HTTPS. Это довольно щекотливый вопрос, требующий коллективных усилий: до тех пор, пока рекламодатели не начнут использовать протокол HTTPS, операторы сайтов не смогут перейти на HTTPS без потери доходов от рекламы; с другой стороны, до тех пор, пока операторы сайтов не перейдут на HTTPS, рекламодатели не видят большой заинтересованности в этом протоколе.

### Performance

Рекламодатели как минимум должны предлагать услуги по рекламе по протоколу HTTPS (например, выполнив инструкции из раздела данного руководства "Включение HTTPS на ваших серверах"). Многие уже так и делают. Рекламодателям, которые вообще не используют HTTPS, следует предложить сделать хотя бы первые шаги на пути к этому. Вы можете отложить выполнение инструкций, приведенных в разделе данного руководства "Создание относительных внутрисайтовых URL-адресов", пока на использование протокола HTTPS не перейдет достаточное количество рекламодателей.

In some cases, TLS can *improve* performance, mostly as a result of making HTTP/2 possible. Chris Palmer gave a talk on [HTTPS and HTTP/2 performance at Chrome Dev Summit 2014](/web/shows/cds/2014/tls-all-the-things).

### Referer headers

When users follow links from your HTTPS site to other HTTP sites, user agents don't send the Referer header. If this is a problem, there are several ways to solve it:

* The other sites should migrate to HTTPS. If referee sites can complete the [Enable HTTPS on your servers](#enable-https-on-your-servers) section of this guide, you can change links in your site to theirs from `http://` to `https://`, or you can use protocol-relative links.
* To work around a variety of problems with Referer headers, use the new [Referrer Policy standard](http://www.w3.org/TR/referrer-policy/#referrer-policy-delivery-meta).

Because search engines are migrating to HTTPS, in the future you are likely see *more* Referer headers when you migrate to HTTPS.

Caution: According to the [HTTP RFC](https://tools.ietf.org/html/rfc2616#section-15.1.3), clients **SHOULD NOT** include a Referer header field in a (non-secure) HTTP request if the referring page is transferred with a secure protocol.

### Ad revenue

Site operators that monetize their site by showing ads want to make sure that migrating to HTTPS does not reduce ad impressions. But due to mixed content security concerns, an HTTP `<iframe>` doesn't work in an HTTPS page. There is a tricky collective action problem here: until advertisers publish over HTTPS, site operators cannot migrate to HTTPS without losing ad revenue; but until site operators migrate to HTTPS, advertisers have little motivation to publish HTTPS.

Advertisers should at least offer ad service via HTTPS (such as by completing the "Enable HTTPS on your servers" section on this page. Many already do. You should ask advertisers that do not serve HTTPS at all to at least start. You may wish to defer completing [Make IntraSite URLs relative](#make-intrasite-urls-relative) until enough advertisers interoperate properly.

## Redirect HTTP to HTTPS

{% include "web/_shared/helpful.html" %}