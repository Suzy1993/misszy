[<< 回到主页](http://suzy1993.github.io/misszy/)

## jQuery 懒加载技术

### 1 懒加载
Lazy Load是一个用JavaScript编写的jQuery插件，可以延迟加载长页面中的图片。在浏览器可视区域外的图片不会被载入，只有当图片出现在浏览器的可视区域内时，才设置图片正真的路径，让图片显示出来。
图片懒加载与图片预加载的处理方式正好相反。

### 2 懒加载的意义
如果一个页面中有成千上万张图片，如淘宝首页等，如果一上来就发送成千上万个加载图片的请求，服务器很可能会吃不消。
懒加载的意义在于：在包含很多大图片的长页面中延迟加载图片，可以加快页面加载速度. 浏览器将会在加载可见图片之后即进入就绪状态，可以帮助降低服务器负担。

### 3 懒加载的使用范围
涉及到图片，falsh，iframe，js文件等占用较大带宽的资源加载时，都可以使用jquery预加载技术。

### 4 懒加载的使用方法
#### 4.1 导入lazyload插件
```
<script src="js/jquery.lazyload.js"></script>
```

#### 4.2 所有图片都延迟加载
```
<script type="text/javascript">
    $("img").lazyload();
</script>
```

#### 4.3 设置敏感度区域
lazyload插件提供了threshold选项，将threshold定为x，表示当可视区域离图片还有200个象素时开始加载图片。
```
$("img").lazyload({ threshold: 200 });
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
