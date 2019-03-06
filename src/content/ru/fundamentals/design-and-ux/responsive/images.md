project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Изображение - важный элемент любой веб-страницы, ведь одна картинка может заменить собой тысячу слов. Проблема в том, что изображения означают дополнительные байты в загруженных файлах. Отзывчивый веб-дизайн предполагает, что в зависимости от параметров устройства могут меняться не только шаблоны страниц, но и сами изображения.

{# wf_updated_on: 2018-12-15 #} {# wf_published_on: 2000-01-01 #}

# Изображения {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Изображение - важный элемент любой веб-страницы, ведь одна картинка может заменить собой тысячу слов. Проблема в том, что изображения означают дополнительные байты в загруженных файлах. Отзывчивый веб-дизайн предполагает изменение в соответствии с параметрами устройства не только для шаблонов страниц, но и для самих изображений.

## Изображения в разметке

<img src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Other times the image may need to be changed more drastically: changing the proportions, cropping, and even replacing the entire image. In this case, changing the image is usually referred to as art direction. See [responsiveimages.org/demos/](https://responsiveimages.org/demos/) for more examples.

В некоторых случаях вам может понадобится радикально оптимизировать изображение: изменить его пропорции, обрезать по краям или даже заменить целиком. Здесь вам поможет техника art direction. Больше примеров вы найдете на этом сайте: [responsiveimages.org/demos/](http://responsiveimages.org/demos/){: .external }.

## Изображения в CSS 

<style>
  .side-by-side {
    display: inline-block;
    margin: 0 20px 0 0;
    width: 45%;
  }

  span#data_uri {
    background: url(data:image/svg+xml,%3C%3Fxml%20version%3D%221.0%22%20encoding%3D%22utf-8%22%3F%3E%0D%0A%3C%21--%20Generator%3A%20Adobe%20Illustrator%2016.0.0%2C%20SVG%20Export%20Plug-In%20.%20SVG%20Version%3A%206.00%20Build%200%29%20%20--%3E%0D%0A%3C%21DOCTYPE%20svg%20PUBLIC%20%22-%2F%2FW3C%2F%2FDTD%20SVG%201.1%2F%2FEN%22%20%22http%3A%2F%2Fwww.w3.org%2FGraphics%2FSVG%2F1.1%2FDTD%2Fsvg11.dtd%22%3E%0D%0A%3Csvg%20version%3D%221.1%22%20id%3D%22Layer_1%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20xmlns%3Axlink%3D%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%22%20x%3D%220px%22%20y%3D%220px%22%0D%0A%09%20width%3D%22396.74px%22%20height%3D%22560px%22%20viewBox%3D%22281.63%200%20396.74%20560%22%20enable-background%3D%22new%20281.63%200%20396.74%20560%22%20xml%3Aspace%3D%22preserve%22%0D%0A%09%3E%0D%0A%3Cg%3E%0D%0A%09%3Cg%3E%0D%0A%09%09%3Cg%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23E44D26%22%20points%3D%22409.737%2C242.502%20414.276%2C293.362%20479.828%2C293.362%20480%2C293.362%20480%2C242.502%20479.828%2C242.502%20%09%09%09%0D%0A%09%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpath%20fill%3D%22%23E44D26%22%20d%3D%22M281.63%2C110.053l36.106%2C404.968L479.757%2C560l162.47-45.042l36.144-404.905H281.63z%20M611.283%2C489.176%0D%0A%09%09%09%09L480%2C525.572V474.03l-0.229%2C0.063L378.031%2C445.85l-6.958-77.985h22.98h26.879l3.536%2C39.612l55.315%2C14.937l0.046-0.013v-0.004%0D%0A%09%09%09%09L480%2C422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283%2C489.176z%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23F16529%22%20points%3D%22480%2C192.833%20604.247%2C192.833%20603.059%2C206.159%20600.796%2C231.338%20599.8%2C242.502%20599.64%2C242.502%20%0D%0A%09%09%09%09480%2C242.502%20480%2C293.362%20581.896%2C293.362%20595.28%2C293.362%20594.068%2C306.699%20582.396%2C437.458%20581.649%2C445.85%20480%2C474.021%20%0D%0A%09%09%09%09480%2C474.03%20480%2C525.572%20611.283%2C489.176%20642.17%2C143.166%20480%2C143.166%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23F16529%22%20points%3D%22540.988%2C343.029%20480%2C343.029%20480%2C422.35%20535.224%2C407.445%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23EBEBEB%22%20points%3D%22414.276%2C293.362%20409.737%2C242.502%20479.828%2C242.502%20479.828%2C242.38%20479.828%2C223.682%20%0D%0A%09%09%09%09479.828%2C192.833%20355.457%2C192.833%20356.646%2C206.159%20368.853%2C343.029%20479.828%2C343.029%20479.828%2C293.362%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23EBEBEB%22%20points%3D%22479.828%2C474.069%20479.828%2C422.4%20479.782%2C422.413%20424.467%2C407.477%20420.931%2C367.864%20%0D%0A%09%09%09%09394.052%2C367.864%20371.072%2C367.864%20378.031%2C445.85%20479.771%2C474.094%20480%2C474.03%20480%2C474.021%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22343.784%2C50.229%20366.874%2C50.229%20366.874%2C75.517%20392.114%2C75.517%20392.114%2C0%20366.873%2C0%20366.873%2C24.938%20%0D%0A%09%09%09%09343.783%2C24.938%20343.783%2C0%20318.544%2C0%20318.544%2C75.517%20343.784%2C75.517%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22425.307%2C25.042%20425.307%2C75.517%20450.549%2C75.517%20450.549%2C25.042%20472.779%2C25.042%20472.779%2C0%20403.085%2C0%20%0D%0A%09%09%09%09403.085%2C25.042%20425.306%2C25.042%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22508.537%2C38.086%20525.914%2C64.937%20526.349%2C64.937%20543.714%2C38.086%20543.714%2C75.517%20568.851%2C75.517%20568.851%2C0%20%0D%0A%09%09%09%09542.522%2C0%20526.349%2C26.534%20510.159%2C0%20483.84%2C0%20483.84%2C75.517%20508.537%2C75.517%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22642.156%2C50.555%20606.66%2C50.555%20606.66%2C0%20581.412%2C0%20581.412%2C75.517%20642.156%2C75.517%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23FFFFFF%22%20points%3D%22480%2C474.021%20581.649%2C445.85%20582.396%2C437.458%20594.068%2C306.699%20595.28%2C293.362%20581.896%2C293.362%20%0D%0A%09%09%09%09480%2C293.362%20479.828%2C293.362%20479.828%2C343.029%20480%2C343.029%20540.988%2C343.029%20535.224%2C407.445%20480%2C422.35%20479.828%2C422.396%20%0D%0A%09%09%09%09479.828%2C422.4%20479.828%2C474.069%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23FFFFFF%22%20points%3D%22479.828%2C242.38%20479.828%2C242.502%20480%2C242.502%20599.64%2C242.502%20599.8%2C242.502%20600.796%2C231.338%20%0D%0A%09%09%09%09603.059%2C206.159%20604.247%2C192.833%20480%2C192.833%20479.828%2C192.833%20479.828%2C223.682%20%09%09%09%22%2F%3E%0D%0A%09%09%3C%2Fg%3E%0D%0A%09%3C%2Fg%3E%0D%0A%3C%2Fg%3E%0D%0A%3C%2Fsvg%3E%0D%0A) no-repeat;
    background-size: cover;
    height: 484px;
  }

  span#svg {
    background: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' version='1.1' x='0px' y='0px' width='50%' height='560px' viewBox='281.63 0 396.74 560' enable-background='new 281.63 0 396.74 560' xml:space='preserve'><g><g><g><polygon fill='#E44D26' points='409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5'/><path fill='#E44D26' d='M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z'/><polygon fill='#F16529' points='480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2'/><polygon fill='#F16529' points='541,343 480,343 480,422.4 535.2,407.4'/><polygon fill='#EBEBEB' points='414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4'/><polygon fill='#EBEBEB' points='479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474'/><polygon points='343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5'/><polygon points='425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25'/><polygon points='508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5'/><polygon points='642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5'/><polygon fill='#FFFFFF' points='480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1'/><polygon fill='#FFFFFF' points='479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7'/></g></g></g></svg>") no-repeat;
    background-size: cover;
    height: 484px;
  }
</style>

 

{% include "web/_shared/udacity/ud882.html" %}

### Отзывчивые изображения

* Указывайте относительные размеры изображения, чтобы оно оставалось в границах контейнера.
* Используйте элемент `picture`, чтобы показывать разные изображения с учетом параметров устройства (эффект art direction).
* Используйте атрибут `srcset` и дескриптор `x` в элементе `img`: при наличии нескольких вариантов браузеру будет проще выбрать нужное изображение.
* If your page only has one or two images and these are not used elsewhere on your site, consider using inline images to reduce file requests.

### Art direction

Элемент `img` выполняет много функций: он загружает, декодирует и визуализирует контент. Современные браузеры поддерживают несколько форматов изображений. Многие изображения работают на разных устройствах, независимо от параметров экрана. Ниже мы расскажем, как добиться наилучших результатов.

Указывая размеры изображения, применяйте относительные величины, чтобы оно не выходило за пределы области просмотра. Например, при параметре width: 50%; изображение в ширину будет занимать 50% вмещаемого элемента (а не области просмотра или актуального размера в пикселях).

    img, embed, object, video {
      max-width: 100%;
    }
    

Технология CSS допускает выход контента за пределы контейнеров. Избежать этого поможет параметр max-width: 100%. Например:

### TL;DR {: .hide-from-toc }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="Pzc5Dly_jEM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Убедитесь, что в элементы img добавлены значимые дескрипторы с помощью атрибута alt. Они создадут контекст для программ для чтения с экрана и других вспомогательных технологий, и в результате пользователям будут легче находить ваш сайт.

<div style="clear:both;">
</div>

    <img src="photo.png" srcset="photo@2x.png 2x" ...>
    

Атрибут `srcset` расширяет функциональные возможности элемента `img`. Благодаря ему вам будет проще назначать изображения с учетом параметров устройства. Как и в случае с `image-set` [(функция CSS)](#use_image-set_to_provide_high_res_images), атрибут `srcset` позволяет браузеру выбирать наиболее подходящее изображение в зависимости от характеристик устройства. Например, использовать изображения 2x на экране 2x и, потенциально, изображения 1x на устройстве 2x при ограниченной пропускной способности сети.

Если браузер не поддерживает атрибут srcset, по умолчанию файл с изображением импортируется с помощью атрибута src. Вот почему так важно включать изображение 1x, которое может отображаться на любых устройствах, независимо от их свойств. Если браузер поддерживает атрибут srcset, вы можете определять список источников изображений и условий (через запятую) до поступления запроса. В результате загружаются и выводятся на экран только те изображения, которые соответствуют параметрам устройства.

### Указывайте относительные размеры изображения

<img class="attempt-right" src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Если вы хотите, чтобы изображения менялись в зависимости от характеристик устройства (эффект art direction), воспользуйтесь элементом picture. Элемент `picture` задает декларативное решение для обеспечения нескольких версий одного изображения в зависимости от различных характеристик устройства: размера, разрешения, назначения и т. д.

<div style="clear:both;">
</div>

Dogfood: The `picture` element is beginning to land in browsers. Although it's not available in every browser yet, we recommend its use because of the strong backward compatibility and potential use of the [Picturefill polyfill](https://scottjehl.github.io/picturefill/). See the [ResponsiveImages.org](http://responsiveimages.org/#implementation) site for further details.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="QINlm3vjnaY"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Note: Элемент `picture` начинает поддерживаться в браузерах. Несмотря на то, что этот элемент поддерживают ещё не все браузеры, мы тем не менее рекомендуем использовать его. Он хорошо совместим с предыдущими версиями, и в дальнейшем его можно будет применять с изображениями [Picturefill polyfill](https://scottjehl.github.io/picturefill/).Дополнительную информацию вы найдете на сайте [ResponsiveImages.org](http://responsiveimages.org/#implementation).

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/media.html" region_tag="picture" %}
</pre>

Используйте элемент `picture`, если у источника изображения несколько вариантов плотности пикселей, а также если в рамках отзывчивого веб-дизайна для некоторых типов экранов назначаются изображения с различающимися характеристиками. Как и в случае с `video`, вы можете указать несколько элементов `source` и назначать разные файлы изображений для разных медиазапросов или форматов изображений.

В приведенном выше примере при ширине браузера не менее 800 пикселей будет использован формат head.jpg или head-2x.jpg (в зависимости от разрешения экрана устройства). Если ширина браузера от 450 до 800 пикселей, применяются форматы head-small.jpg или head-small-2x.jpg (также в зависимости от разрешения экрана устройства). Если речь идет о ширине экрана менее 450 пикселей и устройстве с нисходящей совместимостью, элемент picture поддерживаться не будет. В этом случае для вывода изображения на экран браузер использует элемент img (он должен быть включен).

#### Изображения с относительными размерами

Если финальный размер изображения неизвестен, довольно сложно выбрать дескриптор плотности пикселей для источников изображений. Это, в частности, относится к изображениям, которые растягиваются пропорционально ширине браузера и изменяют свои размеры в зависимости от нее.

Мы советуем не указывать фиксированные размеры изображения и плотность пикселей. Вместо этого вы можете определять размер обрабатываемого изображения, добавив дескриптор width. Это позволит браузеру автоматически вычислить оптимальную плотность пикселей и выбрать корректное изображение для загрузки.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/sizes.html" region_tag="picture" %}
</pre>

Выше приведено изображение, которое занимает половину ширины области просмотра (sizes="50vw") и зависит от ширины браузера и его соотношения логических и физических пикселей. В результате браузер может выбрать изображение, которое будет корректно отображаться в окне любого размера. В приведенной ниже таблице показано, каким может быть этот выбор.

В некоторых случаях размер или изображение могут изменяться в зависимости от точек останова, заданных в шаблоне сайта. Например, вам нужно будет, чтобы на маленьких экранах изображение занимало всю область просмотра, а на экранах более крупного формата достаточно будет небольшой части.

<table class="">
  <tr>
    <th data-th="Browser width">
      Ширина браузера
    </th>
    
    <th data-th="Device pixel ratio">
      Соотношение логических и физических пикселей на устройстве
    </th>
    
    <th data-th="Image used">
      Использованное изображение
    </th>
    
    <th data-th="Effective resolution">
      Эффективная разрешающая способность
    </th>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400 пикселей
    </td>
    
    <td data-th="Device pixel ratio">
      1
    </td>
    
    <td data-th="Image used">
      <code>200.png</code>
    </td>
    
    <td data-th="Effective resolution">
      1x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400 пикселей
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      320 пикселей
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2,5x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      600 пикселей
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>800.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2,67x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      640 пикселей
    </td>
    
    <td data-th="Device pixel ratio">
      3
    </td>
    
    <td data-th="Image used">
      <code>1000.png</code>
    </td>
    
    <td data-th="Effective resolution">
      3,125x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      1100 пикселей
    </td>
    
    <td data-th="Device pixel ratio">
      1
    </td>
    
    <td data-th="Image used">
      <code>1400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      1,27x
    </td>
  </tr>
</table>

#### Создание точек останова в отзывчивых изображениях

В приведенном выше примере атрибут sizes определяет размеры изображения с помощью различных медиазапросов. Если размер окна браузера превышает 600 пикселей, изображение будет занимать 25% области просмотра. При размере браузера от 500 до 600 пикселей это значение увеличивается до 50%, а при размере меньше 500 пикселей изображение становится полноэкранным.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/breakpoints.html" region_tag="picture" %}
</pre>

Люди хотят видеть, что они покупают. Им важно, чтобы на сайтах онлайн-магазинов были представлены крупные изображения товаров в высоком разрешении, чтобы можно было рассмотреть их в деталях. Согласно \[проведенным исследованиям\] (/web/fundamentals/getting-started/principles/), отсутствие такой возможности снижает лояльность покупателей.

Хороший пример изображения, которое увеличивается при нажатии на него, представлен на сайте марки J.Crew. Окно с подсказкой сообщает пользователю, что он может рассмотреть товар во всех деталях, просто нажав на изображение.

### Устройства с экранами высокого разрешения: добавьте к элементу img атрибут srcset<figure class="attempt-right"> 

<img src="img/sw-make-images-expandable-good.png" srcset="img/sw-make-images-expandable-good.png 1x, img/sw-make-images-expandable-good-2x.png 2x" alt="Веб-сайт марки J.Crew с масштабируемым изображением товара" /> <figcaption>Веб-сайт марки J.Crew с масштабируемым изображением товара</figcaption> </figure> 

Техника [сжатия изображений](http://www.html5rocks.com/en/mobile/high-dpi/#toc-tech-overview) предназначена для высокого сжатия изображений 2x при передаче на любые устройства, независимо от их параметров. В зависимости от типа изображения и степени сжатия качество изображения может не претерпеть заметных изменений, однако размер файла значительно уменьшится.

A good example of tappable, expandable images is provided by the J. Crew site. A disappearing overlay indicates that an image is tappable, providing a zoomed in image with fine detail visible.

<div style="clear:both;">
</div>

### Эффект art direction в отзывчивых изображениях с элементом picture

#### Сжатые изображения

Note: С осторожностью используйте техники сжатия, так как они занимают больше памяти и требуют затрат на декодирование. Адаптация крупных изображений для небольших экранов - ресурсоемкий процесс. Он может вызвать проблемы в работе низкопроизводительных устройств с небольшим объемом памяти и ограниченными возможностями обработки данных.

Техника замещения текста изображением в JavaScript выявляет характеристики устройства и предлагает оптимальное решение. Вы можете определить соотношение логических и физических пикселей через величину window.devicePixelRatio, задать ширину и высоту экрана и, потенциально, установить проверку сетевого соединения через navigator.connection или с помощью поддельного запроса. Когда вся необходимая информация будет собрана, вы сможете выбрать изображение для загрузки.

Основной недостаток этой техники в том, что загрузка изображения приостанавливается до окончания работы анализатора предпросмотра. Это значит, что загрузка изображений начнется только после завершения события pageload. Кроме того, браузер с большой вероятностью загрузит изображение обоих видов, 1x и 2x, и в результате вес страницы увеличится.

#### Замена текста изображением в JavaScript

Группа свойств CSS background позволяет добавлять в элементы комплексные изображения, облегчает добавление сразу нескольких изображений, обеспечивает их повторяемость их и т. д. В сочетании с медиазапросами background обеспечивает условную загрузку изображений с учетом разрешения экрана, размеров области просмотра и т. д.

Медиазапросы оказывают влияние не только на шаблон страницы, их также можно использовать для условной загрузки изображений и создания эффекта art direction в соответствии с размером области просмотра.

#### Inlining images: raster and vector

В приведенном ниже примере видно, что на экранах небольшого размера загружен только элемент small.png, он же применен к div. На более крупных экранах свойство background-image: url(body.png) применено к элементу body, а атрибут background-image: url(large.png) применен к div.

Функция image-set технологии CSS дополняет группу свойств background: с ее помощью вам будет проще задать несколько изображений для устройств с разными параметрами. Браузер выберет тот файл, который больше всего подходит для данного устройства с учетом его характеристик. Например, применит 2x изображение для экрана 2x и 1x изображение для экрана 2x, если пропускная способность сети ограничена.

Но это ещё не все. Браузер не только загрузит наиболее подходящее изображение, но и пропорционально изменит его размер с учетом всех требований. Иными словами, браузер `поймет`, что изображение 2x в два раза тяжелее изображения 1x, и пропорционально уменьшит первое изображение в 2 раза, чтобы сохранить его размер на странице.

##### SVG

Свойство image-set() было введено недавно и поддерживается только в браузерах Chrome и Safari с префиксом -webkit. Необходимо также предусмотреть резервное изображение на случай, если image-set() не поддерживается. Например:

В приведенном выше примере соответствующий цифровой объект будет загружен в браузерах, поддерживающих image-set. В остальных случаях будет использован объект 1x. В настоящее время только немногие браузеры поддерживают image-set(), поэтому, как правило, используются объекты с разрешением 1x.

<img class="side-by-side" src="img/html5.png" alt="HTML5 logo, PNG format" /> <img class="side-by-side" src="img/html5.svg" alt="HTML5 logo, SVG format" />

Браузеры Chrome, Firefox и Opera поддерживают стандарт (`min-resolution: 2dppx`). Для браузеров Safari и Android требуются более старые варианты синтаксиса, а которых отсутствует единица dppx. Обратите внимание, что загрузка стилей возможна только в том случае, если параметры устройства соответствуют медиазапросу. В остальных случаях вам потребуется также назначить стили. Таким образом вы сможете задать изображение, которое будет выводиться на экран, если браузер не поддерживает медиазапросы, касающиеся разрешения устройства.

<img class="side-by-side" src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiB
      BZG9iZSBJbGx1c3RyYXRvciAxNi4wLjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW
      9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RUR
      CBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2
      ZzExLmR0ZCI+DQo8c3ZnIHZlcnNpb249IjEuMSIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8
      vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OT
      kveGxpbmsiIHg9IjBweCIgeT0iMHB4Ig0KCSB3aWR0aD0iMzk2Ljc0cHgiIGhlaWdodD0iNTYwc
      HgiIHZpZXdCb3g9IjI4MS42MyAwIDM5Ni43NCA1NjAiIGVuYWJsZS1iYWNrZ3JvdW5kPSJuZXcg
      MjgxLjYzIDAgMzk2Ljc0IDU2MCIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSINCgk+DQo8Zz4NCgk8Zz4
      NCgkJPGc+DQoJCQk8cG9seWdvbiBmaWxsPSIjRTQ0RDI2IiBwb2ludHM9IjQwOS43MzcsMjQyLj
      UwMiA0MTQuMjc2LDI5My4zNjIgNDc5LjgyOCwyOTMuMzYyIDQ4MCwyOTMuMzYyIDQ4MCwyNDIuN
      TAyIDQ3OS44MjgsMjQyLjUwMiAJCQkNCgkJCQkiLz4NCgkJCTxwYXRoIGZpbGw9IiNFNDREMjYi
      IGQ9Ik0yODEuNjMsMTEwLjA1M2wzNi4xMDYsNDA0Ljk2OEw0NzkuNzU3LDU2MGwxNjIuNDctNDU
    uMDQybDM2LjE0NC00MDQuOTA1SDI4MS42M3ogTTYxMS4yODMsNDg5LjE3Ng0KCQkJCUw0ODAsNT
    I1LjU3MlY0NzQuMDNsLTAuMjI5LDAuMDYzTDM3OC4wMzEsNDQ1Ljg1bC02Ljk1OC03Ny45ODVoM
    jIuOThoMjYuODc5bDMuNTM2LDM5LjYxMmw1NS4zMTUsMTQuOTM3bDAuMDQ2LTAuMDEzdi0wLjAw
    NA0KCQkJCUw0ODAsNDIyLjM1di03OS4zMmgtMC4xNzJIMzY4Ljg1M2wtMTIuMjA3LTEzNi44NzF
    sLTEuMTg5LTEzLjMyNWgxMjQuMzcxSDQ4MHYtNDkuNjY4aDE2Mi4xN0w2MTEuMjgzLDQ4OS4xNz
    Z6Ii8+DQoJCQk8cG9seWdvbiBmaWxsPSIjRjE2NTI5IiBwb2ludHM9IjQ4MCwxOTIuODMzIDYwN
    C4yNDcsMTkyLjgzMyA2MDMuMDU5LDIwNi4xNTkgNjAwLjc5NiwyMzEuMzM4IDU5OS44LDI0Mi41
    MDIgNTk5LjY0LDI0Mi41MDIgDQoJCQkJNDgwLDI0Mi41MDIgNDgwLDI5My4zNjIgNTgxLjg5Niw
    yOTMuMzYyIDU5NS4yOCwyOTMuMzYyIDU5NC4wNjgsMzA2LjY5OSA1ODIuMzk2LDQzNy40NTggNT
    gxLjY0OSw0NDUuODUgNDgwLDQ3NC4wMjEgDQoJCQkJNDgwLDQ3NC4wMyA0ODAsNTI1LjU3MiA2M
    TEuMjgzLDQ4OS4xNzYgNjQyLjE3LDE0My4xNjYgNDgwLDE0My4xNjYgCQkJIi8+DQoJCQk8cG9s
    eWdvbiBmaWxsPSIjRjE2NTI5IiBwb2ludHM9IjU0MC45ODgsMzQzLjAyOSA0ODAsMzQzLjAyOSA
    0ODAsNDIyLjM1IDUzNS4yMjQsNDA3LjQ0NSAJCQkiLz4NCgkJCTxwb2x5Z29uIGZpbGw9IiNFQk
    VCRUIiIHBvaW50cz0iNDE0LjI3NiwyOTMuMzYyIDQwOS43MzcsMjQyLjUwMiA0NzkuODI4LDI0M
    i41MDIgNDc5LjgyOCwyNDIuMzggNDc5LjgyOCwyMjMuNjgyIA0KCQkJCTQ3OS44MjgsMTkyLjgz
    MyAzNTUuNDU3LDE5Mi44MzMgMzU2LjY0NiwyMDYuMTU5IDM2OC44NTMsMzQzLjAyOSA0NzkuODI
    4LDM0My4wMjkgNDc5LjgyOCwyOTMuMzYyIAkJCSIvPg0KCQkJPHBvbHlnb24gZmlsbD0iI0VCRU
    JFQiIgcG9pbnRzPSI0NzkuODI4LDQ3NC4wNjkgNDc5LjgyOCw0MjIuNCA0NzkuNzgyLDQyMi40M
    TMgNDI0LjQ2Nyw0MDcuNDc3IDQyMC45MzEsMzY3Ljg2NCANCgkJCQkzOTQuMDUyLDM2Ny44NjQg
    MzcxLjA3MiwzNjcuODY0IDM3OC4wMzEsNDQ1Ljg1IDQ3OS43NzEsNDc0LjA5NCA0ODAsNDc0LjA
    zIDQ4MCw0NzQuMDIxIAkJCSIvPg0KCQkJPHBvbHlnb24gcG9pbnRzPSIzNDMuNzg0LDUwLjIyOS
    AzNjYuODc0LDUwLjIyOSAzNjYuODc0LDc1LjUxNyAzOTIuMTE0LDc1LjUxNyAzOTIuMTE0LDAgM
    zY2Ljg3MywwIDM2Ni44NzMsMjQuOTM4IA0KCQkJCTM0My43ODMsMjQuOTM4IDM0My43ODMsMCAz
    MTguNTQ0LDAgMzE4LjU0NCw3NS41MTcgMzQzLjc4NCw3NS41MTcgCQkJIi8+DQoJCQk8cG9seWd
    vbiBwb2ludHM9IjQyNS4zMDcsMjUuMDQyIDQyNS4zMDcsNzUuNTE3IDQ1MC41NDksNzUuNTE3ID
    Q1MC41NDksMjUuMDQyIDQ3Mi43NzksMjUuMDQyIDQ3Mi43NzksMCA0MDMuMDg1LDAgDQoJCQkJN
    DAzLjA4NSwyNS4wNDIgNDI1LjMwNiwyNS4wNDIgCQkJIi8+DQoJCQk8cG9seWdvbiBwb2ludHM9
    IjUwOC41MzcsMzguMDg2IDUyNS45MTQsNjQuOTM3IDUyNi4zNDksNjQuOTM3IDU0My43MTQsMzg
    uMDg2IDU0My43MTQsNzUuNTE3IDU2OC44NTEsNzUuNTE3IDU2OC44NTEsMCANCgkJCQk1NDIuNT
    IyLDAgNTI2LjM0OSwyNi41MzQgNTEwLjE1OSwwIDQ4My44NCwwIDQ4My44NCw3NS41MTcgNTA4L
    jUzNyw3NS41MTcgCQkJIi8+DQoJCQk8cG9seWdvbiBwb2ludHM9IjY0Mi4xNTYsNTAuNTU1IDYw
    Ni42Niw1MC41NTUgNjA2LjY2LDAgNTgxLjQxMiwwIDU4MS40MTIsNzUuNTE3IDY0Mi4xNTYsNzU
    uNTE3IAkJCSIvPg0KCQkJPHBvbHlnb24gZmlsbD0iI0ZGRkZGRiIgcG9pbnRzPSI0ODAsNDc0Lj
    AyMSA1ODEuNjQ5LDQ0NS44NSA1ODIuMzk2LDQzNy40NTggNTk0LjA2OCwzMDYuNjk5IDU5NS4yO
    CwyOTMuMzYyIDU4MS44OTYsMjkzLjM2MiANCgkJCQk0ODAsMjkzLjM2MiA0NzkuODI4LDI5My4z
    NjIgNDc5LjgyOCwzNDMuMDI5IDQ4MCwzNDMuMDI5IDU0MC45ODgsMzQzLjAyOSA1MzUuMjI0LDQ
    wNy40NDUgNDgwLDQyMi4zNSA0NzkuODI4LDQyMi4zOTYgDQoJCQkJNDc5LjgyOCw0MjIuNCA0Nz
    kuODI4LDQ3NC4wNjkgCQkJIi8+DQoJCQk8cG9seWdvbiBmaWxsPSIjRkZGRkZGIiBwb2ludHM9I
    jQ3OS44MjgsMjQyLjM4IDQ3OS44MjgsMjQyLjUwMiA0ODAsMjQyLjUwMiA1OTkuNjQsMjQyLjUw
    MiA1OTkuOCwyNDIuNTAyIDYwMC43OTYsMjMxLjMzOCANCgkJCQk2MDMuMDU5LDIwNi4xNTkgNjA
    0LjI0NywxOTIuODMzIDQ4MCwxOTIuODMzIDQ3OS44MjgsMTkyLjgzMyA0NzkuODI4LDIyMy42OD
    IgCQkJIi8+DQoJCTwvZz4NCgk8L2c+DQo8L2c+DQo8L3N2Zz4NCg==" /> <svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg>

Чтобы добавить значки на веб-страницу, воспользуйтесь SVG-графикой или символами Unicode.

<svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg><svg class="side-by-side" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" width="50%" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5"/><path fill="#E44D26" d="M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z"/><polygon fill="#F16529" points="480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2"/><polygon fill="#F16529" points="541,343 480,343 480,422.4 535.2,407.4"/><polygon fill="#EBEBEB" points="414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4"/><polygon fill="#EBEBEB" points="479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474"/><polygon points="343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5"/><polygon points="425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25"/><polygon points="508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5"/><polygon points="642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5"/><polygon fill="#FFFFFF" points="480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1"/><polygon fill="#FFFFFF" points="479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7"/></g></g></g></svg>

##### Data URI

Кроме классического буквенного набора, в Unicode предусмотрены символы для обозначения числовых диаграмм (&#8528;), стрелок (&#8592;), математических символов (&#8730;), геометрических фигур (&#9733;), кнопок управления (&#9654;), символов шрифта Брайля (&#10255;), нот (&#9836;), букв греческого алфавита (&#937;) и даже шахманых фигур (&#9822;).

    <img src="data:image/svg+xml;base64,[data]">
    

Как и именованные объекты, символы Unicode имеют следующий вид: `&#XXXX;`, где XXXX - это номер символа в Unicode. Например:

    background-image: image-set(
      url(icon1x.jpg) 1x,
      url(icon2x.jpg) 2x
    );
    

Ты - супер &#9733;

Как правило, значки SVG мало весят и просты в использовании. Им также можно задавать стили с помощью CSS. У этих значков есть ряд преимуществ перед растровыми изображениями:

##### Inlining in CSS

Data URIs and SVGs can also be inlined in CSS&mdash;and this is supported on both mobile and desktop. Here are two identical-looking images implemented as background images in CSS; one Data URI, one SVG:

<span class="side-by-side" id="data_uri"></span> <span class="side-by-side" id="svg"></span>

##### Inlining pros & cons

Inline code for images can be verbose&mdash;especially Data URIs&mdash;so why would you want to use it? To reduce HTTP requests! SVGs and Data URIs can enable an entire web page, including images, CSS and JavaScript, to be retrieved with one single request.

Существует множество иконочных шрифтов (как платных, так и бесплатных), например [Font Awesome](http://fortawesome.github.io/Font-Awesome/), [Pictos](http://pictos.cc/){: .external } или [Glyphicons](http://glyphicons.com/).

* Используйте изображение, которое оптимально подходит под парамеры устройства: размер экрана, разрешение и шаблон страниц.
* Замените свойство CSS `background-image` для экранов с высоким разрешением и применением медиазапросов на `min-resolution` и `-webkit-min-device-pixel-ratio`.
* Для изображений в высоком разрешении используйте атрибут srcset (наряду с изображениями 1x в разметке).
* Учитывайте эффективность затрат при использовании техники замещения текста изображением в JavaScript или сжатии изображений высокого разрешения для устройств с низким разрешением экрана.
* Data URIs cannot be cached, so must be downloaded for every page they're used on.
* They're not supported in IE 6 and 7, incomplete support in IE8.
* With HTTP/2, reducing the number of asset requests will become less of a priority.

Объем дополнительных HTTP-запросов и размер файла должны соответствовать поставленной задаче. Например, если вам достаточно небольшого количества значков, возможно, рациональнее будет использовать отдельные изображения или спрайт.

## Используйте SVG-графику для создания значков

Изображения часто добавляют вес загруженным файлам, а также занимают значительную площадь визуального пространства страницы. Оптимизировав изображения, вы сможете сэкономить байты и повысить производительность вашего веб-сайта. Чем меньше байтов требуется загрузить браузеру, тем меньше нагрузка на пропускную способность сети клиента и тем быстрее браузер скачивает и отображает все цифровые объекты.

### Используйте масштабируемое изображение товара

* Для создания значков вместо растровых изображений используйте SVG-графику или символы Unicode.
* Change the `background-image` property in CSS for high DPI displays using media queries with `min-resolution` and `-webkit-min-device-pixel-ratio`.
* Use srcset to provide high resolution images in addition to the 1x image in markup.
* Consider the performance costs when using JavaScript image replacement techniques or when serving highly compressed high resolution images to lower resolution devices.

### Другие способы публикации изображений

Мы имеем дело с изображениями двух типов: [векторными](http://ru.wikipedia.org/wiki/Векторная_графика) и [растровыми](http://ru.wikipedia.org/wiki/Растровая_графика). При работе с растровой графикой вам потребуется выбрать корректный формат сжатия, например GIF, PNG или JPG.

**Растровые изображения** - это, как правило, фотографии и другие изображения, которые представляют собой сетку пикселей или точек. Обычно они создаются с помощью фотоаппаратов, сканеров, а также в браузере (элемент canvas). Поскольку размер изображения становится больше, размер файла также увеличивается. Если размер масштабированного изображения больше исходного, растровые изображения становятся размытыми, так как браузер не может правильно заполнить недостающие пиксели.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/image-set.html" region_tag="imageset" %}
</pre>

**Векторная графика** - это, как правило, логотипы и штриховые рисунки. Представляет собой серию кривых, линий, фигур и цветов закраски. Векторные изображения создаются с помощью таких программ, как Adobe Illustrator или Inkscape, и сохраняются в формате [SVG](http://css-tricks.com/using-svg/). Они состоят из простых элементов, поэтому их можно масштабировать без потери качества. Размер файла также остается прежним.

### TL;DR {: .hide-from-toc }

При выборе формата важно учитывать как тип изображения (растровое или векторное), так и его контент (цвета, анимация, текст и т. д.). Не существует универсальных решений, которые можно использовать для всех типов изображений с одинаковой эффективностью. У всех форматов есть свои сильные и слабые стороны.

    @media (min-resolution: 2dppx),
    (-webkit-min-device-pixel-ratio: 2)
    {
      /* High dpi styles & resources here */
    }
    

Ниже мы расскажем о том, как выбрать наиболее подходящий формат в каждом конкретном случае.

Вы можете значительно уменьшить размер файла, если обработаете его после сохранения. Существует множество инструментов сжатия изображений: для форматов lossy и lossless, веб-страниц, графических интерфейсов, командной строки и т. д. По возможности применяйте автоматическую оптимизацию изображений, это очень поможет вам в работе.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" %}
</pre>

Некоторые инструменты преобразуют файлы JPG и PNG в формат lossless без потери качества изображения. Для работы с файлами JPG мы рекомендуем утилиты [jpegtran](http://jpegclub.org/){: .external } и [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (только для Linux, запускается с функцией --strip-all). Для оптимизации файлов PNG воспользуйтесь [OptiPNG](http://optipng.sourceforge.net/) или [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

CSS-спрайтинг представляет собой технику объединения нескольких изображений в одно - так называемый лист спрайта. Вот как отдельные изображения выводятся на экран: фоновое изображение настраивается только для одного элемента (листа спрайта), а затем корректируется таким образом, чтобы на экране отображалась только нужная часть фона.

### Используйте медиазапросы для условной загрузки изображений и art direction

Media queries can create rules based on the [device pixel ratio](http://www.html5rocks.com/en/mobile/high-dpi/#toc-bg), making it possible to specify different images for 2x versus 1x displays.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

Благодаря спрайтингу сокращается число загрузок изображений при включенном сохранении в кеше.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" adjust_indentation="auto" %}
</pre>

Отложенная загрузка может значительно ускорить работу длинных веб-страниц с большим количеством изображений в свернутых вкладках. Загрузка изображений происходит по мере необходимости или после загрузки и вывода на экран базового контента. Эта техника не только повышает эффективность работы веб-сайта, но и может быть использована для создания страниц с неограниченной постраничной прокруткой.

Обратите внимание, что в последнем случае контент загружается только при попадании в область просмотра и, соответственно, может быть недоступен для поисковых систем. Кроме того, пользователи не смогут добраться до нижнего колонтитула страницы из-за постоянной подгрузки контента.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

## Оптимизация изображений для эффективной работы сайта

Картинка совсем не обязательно должна быть именно изображением. Иногда нужного эффекта можно добиться с помощью встроенных параметров браузера. Браузер создает визуальный ряд там, где раньше требовались изображения. Таким образом, ему больше не нужно скачивать отдельные файлы изображений и корректировать ошибки масштабирования. Чтобы добавить на страницу значки, воспользуйтесь символами Unicode или специальными иконочными шрифтами.

### Используйте функцию image-set для изображений с высоким разрешением

* Они относятся к векторной графике, что дает бесконечные возможности для масштабирования.

### Используйте медиазапросы для назначения изображений с высоким разрешением и эффекта art direction

По возможности текст должен быть представлен в виде текста и не встраиваться в изображение. Например, мы не советуем использовать изображения в колонтитулах или размещать внутри картинок контактную информацию (телефоны, адрес а). Такие данные невозможно скопировать, они недоступны для программ чтения с экрана, а также неадаптивны. Вместо этого поместите текст в разметку и при необходимости используйте веб-шрифты, чтобы задать нужные стили.

Современные браузеры используют стили CSS в тех случаях, для которых ранее требовались изображения. Например, комплексный градиент можно создать с помощью группы свойств `background`, тени - с помощью `box-shadow`, а для скругленных углов есть свойство `border-radius`.

Including a unicode character is done in the same way named entities are: `&#XXXX`, where `XXXX` represents the unicode character number. For example:

    You're a super &#9733;
    

Обратите внимание, что для перечисленных техник требуется визуализация Cycles (это может быть важно при работе с мобильными устройствами). Их неадекватное использование приведет к потере положительных результатов и снижению эффективности.

### TL;DR {: .hide-from-toc }

For more complex icon requirements, SVG icons are generally lightweight, easy to use, and can be styled with CSS. SVG have a number of advantages over raster images:

* Также относятся к векторной графике и могут масштабироваться без ограничений. Однако в результате сглаживания значки могут оказаться недостаточно четкими.
* Ограниченные возможности для применения стилей CSS. *Оптимальное расположение пикселей может быть нарушено высотой строки, интервалом между знаками и т. д.
* Иконочные шифты не семантичны, поэтому плохо подходят для устройств для чтения с экрана и других подобных технологий.
* Могут привести к тому, что в файлах большого размера будут применяться только уменьшенные варианты значков (кроме ситуаций непосредственного просмотра).
* They provide better accessibility with the appropriate attributes.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-svg.html){: target="_blank" .external }

### Символы Unicode вместо простых значков<figure class="attempt-right"> 

<img src="img/icon-fonts.png" class="center" srcset="img/icon-fonts.png 1x, img/icon-fonts-2x.png 2x" alt="Example of a page that uses FontAwesome for its font icons." /> <figcaption> <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html" target="_blank" class="external"> Example of a page that uses FontAwesome for its font icons. </a> </figcaption> </figure> 

Icon fonts are popular, and can be easy to use, but have some drawbacks compared to SVG icons:

* Внимательно отнеситесь к формату изображения. Изучите доступные вам форматы и выберите наиболее подходящий.
* Чтобы уменьшить размер файла, воспользуйтесь инструментами для оптимизации и сжатия изображений.
* Сократите число HTTP-запросов. Для этого объедините часто запрашиваемые изображения в спрайты.
* Загрузка изображений должна начинаться, когда они попадают в область просмотра. Так вы ускорите загрузку базовой страницы и уменьшите ее вес.
* Unless properly scoped, they can result in a large file size for only using a small subset of the icons available.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/icon-font.html" region_tag="iconfont" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html){: target="_blank" .external }

There are hundreds of free and paid icon fonts available including [Font Awesome](https://fortawesome.github.io/Font-Awesome/), [Pictos](http://pictos.cc/){: .external }, and [Glyphicons](https://glyphicons.com/).

Be sure to balance the weight of the additional HTTP request and file size with the need for the icons. For example, if you only need a handful of icons, it may be better to use an image or an image sprite.

## Старайтесь не использовать изображения

Images often account for most of the downloaded bytes and also often occupy a significant amount of the visual space on the page. As a result, optimizing images can often yield some of the largest byte savings and performance improvements for your website: the fewer bytes the browser has to download, the less competition there is for client's bandwidth and the faster the browser can download and display all the assets.

### SVG-графика вместо комплексных значков

* Для фото используйте формат JPG.
* Для создания векторной графики и сплошной заливки (например, логотипов и штриховых рисунков) подходит SVG. Если векторные изображения недоступны, попробуйте WebP или PNG.
* Чтобы отобразить больше цветов и добиться лучшего коэффициента сжатия, выбирайте PNG вместо GIF.
* Для роликов используйте тег `<video>`. В результате вы получите более качественное изображение, а пользователь сможет управлять воспроизведением.

### Осторожнее с иконочными шрифтами

There are two types of images to consider: [vector images](https://en.wikipedia.org/wiki/Vector_graphics) and [raster images](https://en.wikipedia.org/wiki/Raster_graphics). For raster images, you also need to choose the right compression format, for example: `GIF`, `PNG`, `JPG`.

**Raster images**, like photographs and other images, are represented as a grid of individual dots or pixels. Raster images typically come from a camera or scanner, or can be created in the browser with the `canvas` element. As the image size gets larger, so does the file size. When scaled larger than their original size, raster images become blurry because the browser needs to guess how to fill in the missing pixels.

**Vector images**, such as logos and line art, are defined by a set of curves, lines, shapes, and fill colors. Vector images are created with programs like Adobe Illustrator or Inkscape and saved to a vector format like [`SVG`](https://css-tricks.com/using-svg/). Because vector images are built on simple primitives, they can be scaled without any loss in quality or change in file size.

When choosing the appropriate format, it is important to consider both the origin of the image (raster or vector), and the content (colors, animation, text, etc). No one format fits all image types, and each has its own strengths and weaknesses.

Start with these guidelines when choosing the appropriate format:

* Use `JPG` for photographic images.
* Use `SVG` for vector art and solid color graphics such as logos and line art. If vector art is unavailable, try `WebP` or `PNG`.
* Use `PNG` rather than `GIF` as it allows for more colors and offers better compression ratios.
* For longer animations consider using `<video>`, which provides better image quality and gives the user control over playback.

### TL;DR {: .hide-from-toc }

You can reduce image file size considerably by "post-processing" the images after saving. There are a number of tools for image compression&mdash;lossy and lossless, online, GUI, command line. Where possible, it's best to try automating image optimization so that it's a first-class citizen in your workflow.

Several tools are available that perform further, lossless compression on `JPG` and `PNG` files with no effect on image quality. For `JPG`, try [jpegtran](http://jpegclub.org/) or [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (available on Linux only; run with the --strip-all option). For `PNG`, try [OptiPNG](http://optipng.sourceforge.net/) or [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

### Выбирайте подходящий формат

<img src="img/sprite-sheet.png" class="attempt-right" alt="Image sprite sheet used in example" />

CSS spriting is a technique whereby a number of images are combined into a single "sprite sheet" image. You can then use individual images by specifying the background image for an element (the sprite sheet) plus an offset to display the correct part.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/image-sprite.html){: target="_blank" .external }

Spriting has the advantage of reducing the number of downloads required to get multiple images, while still enabling caching.

### Уменьшите размер файла

Lazy loading can significantly speed up loading on long pages that include many images below the fold by loading them either as needed or when the primary content has finished loading and rendering. In addition to performance improvements, using lazy loading can create infinite scrolling experiences.

Be careful when creating infinite scrolling pages&mdash;because content is loaded as it becomes visible, search engines may never see that content. In addition, users who are looking for information they expect to see in the footer, never see the footer because new content is always loaded.

## Avoid images completely

Sometimes the best image isn't actually an image at all. Whenever possible, use the native capabilities of the browser to provide the same or similar functionality. Browsers generate visuals that would have previously required images. This means that browsers no longer need to download separate image files thus preventing awkwardly scaled images. You can use unicode or special icon fonts to render icons.

### Используйте CSS-спрайты

Wherever possible, text should be text and not embedded into images. For example, using images for headlines or placing contact information&mdash;like phone numbers or addresses&mdash;directly into images prevents users from copying and pasting the information; it makes the information inaccessible for screen readers, and it isn't responsive. Instead, place the text in your markup and if necessary use webfonts to achieve the style you need.

### Применяйте отложенную загрузку

Modern browsers can use CSS features to create styles that would previously have required images. For example: complex gradients can be created using the `background` property, shadows can be created using `box-shadow`, and rounded corners can be added with the `border-radius` property. 

<style>
  p#noImage {
    margin-top: 2em;
    padding: 1em;
    padding-bottom: 2em;
    color: white;
    border-radius: 5px;
    box-shadow: 5px 5px 4px 0 rgba(9,130,154,0.2);
    background: linear-gradient(rgba(9, 130, 154, 1), rgba(9, 130, 154, 0.5));
  }

  p#noImage code {
    color: rgb(64, 64, 64);
  }
</style>

 

<p id="noImage">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque sit amet augue eu magna scelerisque porta ut ut dolor. Nullam placerat egestas nisl sed sollicitudin. Fusce placerat, ipsum ac vestibulum porta, purus dolor mollis nunc, pharetra vehicula nulla nunc quis elit. Duis ornare fringilla dui non vehicula. In hac habitasse platea dictumst. Donec ipsum lectus, hendrerit malesuada sapien eget, venenatis tempus purus.
</p>

    <style>
      div#noImage {
        color: white;
        border-radius: 5px;
        box-shadow: 5px 5px 4px 0 rgba(9,130,154,0.2);
        background: linear-gradient(rgba(9, 130, 154, 1), rgba(9, 130, 154, 0.5));
      }
    </style>
    

Keep in mind that using these techniques does require rendering cycles, which can be significant on mobile. If over-used, you'll lose any benefit you may have gained and it may hinder performance.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}