---
title: 一言(Hitokoto)
date: 2017-11-15
categories: [Web]
tags: Web
---

## 关于一言

近日发现了一个有趣的网站 [一言](hitokoto.cn). 并提供API. 所以, 当然是玩一玩啦.

> 一言网(Hitokoto.cn)创立于2016年，隶属于萌创Team，目前网站主要提供一句话服务。
>
> 动漫也好、小说也好、网络也好，不论在哪里，我们总会看到有那么一两个句子能穿透你的心。我们把这些句子汇聚起来，形成一言网络，以传递更多的感动。如果可以，我们希望我们没有停止服务的那一天。

## 调用

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>一言Hitokoto</title>
  <style>
    blockquote{
     border-left: .1em solid gray;
     padding-left: 15px;
   }
  </style>
  <link rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/styles/default.min.css">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/highlight.min.js"></script>
  <script>hljs.initHighlightingOnLoad();</script>
</head>

<body>
  <blockquote id="whisper">
    <script>
      fetch('https://sslapi.hitokoto.cn/?encode=text')
      .then(x => x.text())
      .then(x => document.getElementById('whisper').innerHTML = x)
    </script>
  </blockquote>

  <pre>
    <code class="html">
      &lt;blockquote id="whisper"&gt;
        &lt;script&gt;
          fetch('https://sslapi.hitokoto.cn/?encode=text')
          .then(x => x.text())
          .then(x => document.getElementById('whisper').innerHTML = x)
        &lt;/script&gt;
      &lt;/blockquote&gt; &nbsp;
    </code>
  </pre>
</body>

</html>
```

