# public根目录文件如何引用

html文件中引用方式：%PUBLIC\_URL%，如：

```
<link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
<script src="%PUBLIC_URL%/browser-polyfill.min.js"></script>
<script src="%PUBLIC_URL%/config.js"></script>
```

在package.json中添加`"homepage":"."`



`//js中引用方式：`



