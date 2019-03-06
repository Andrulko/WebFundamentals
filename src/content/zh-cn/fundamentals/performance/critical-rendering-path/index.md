project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description:优化关键渲染路径是指优先显示与当前用户操作有关的内容。

{# wf_updated_on: 2015-10-05 #} {# wf_published_on: 2014-03-31 #}

# 关键渲染路径 {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*优化关键渲染路径*是指优先显示与当前用户操作有关的内容。

要提供快速的网络体验，浏览器需要做许多工作。这类工作大多数是我们这些网络开发者看不到的：我们编写标记，屏幕上就会显示出漂亮的页面。

但浏览器到底是如何使用我们的 HTML、CSS 和 JavaScript 在屏幕上渲染像素的呢？

<img src="images/progressive-rendering.png"  alt="progressive page rendering" />

By optimizing the critical rendering path we can significantly improve the time to first render of our pages. Further, understanding the critical rendering path also serves as a foundation for building well-performing interactive applications. The interactive updates process is the same, just done in a continuous loop and ideally at 60 frames per second! But first, an overview of how the browser displays a simple page.

通过优化关键渲染路径，我们可以显著缩短首次渲染页面的时间。 此外，了解关键渲染路径还可以为构建高性能交互式应用打下基础。

## Feedback {: #feedback }

处理交互式更新的过程是相同的，只是在连续循环中完成，理想情况下每秒可以处理 60 帧！不过，我们先来看一下浏览器如何显示简单的网页。