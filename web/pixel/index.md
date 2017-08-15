[<< 回到主页](http://suzy1993.github.io/misszy/)

## 移动端开发——物理像素和逻辑像素

### 1 物理像素
设备像素，在同一个设备上，它的物理像素是固定的，这是厂商在出厂时就设置好了的，即一个设备的分辨率是固定的。

### 2 逻辑像素
CSS像素，viewport中的一个小方格，CSS样式代码中使用的是逻辑像素。如果在一个设备中，物理像素与逻辑像素相等，将不会产生任何问题。但是，在iphone 4中，物理像素是640px*960px，而逻辑像素数为320*480px。因此，需要使用大约4个物理像素来显示一个CSS像素。

### 3 像素比
物理像素与逻辑像素之间的比例。当像素比为1:1时，使用1个物理像素显示1个逻辑像素；当像素比为2:1时，使用4个物理像素显示1个逻辑像素。

### 4 CSS中的1px并不等于设备的1px
在CSS中一般使用px作为单位，在Web浏览器中CSS的1个像素往往都是对应着电脑屏幕的1个物理像素，这可能会造成一个错觉，那就是CSS中的像素就是设备的物理像素。但实际情况却并非如此，CSS中的像素只是一个抽象的单位，在不同的设备或不同的环境中，CSS中的1px所代表的设备物理像素是不同的。
在早先的移动设备中，屏幕像素密度都比较低，如iphone3，它的分辨率为320*480，在iphone3上，1个CSS像素确实是等于1个物理像素的。后来随着技术的发展，移动设备的像素越来越高，从iphone4开始，推出了所谓的Retina屏，分辨率提高了一倍，变成640*960，但屏幕尺寸却没变化，这就意味着同样大小的屏幕上，像素却多了一倍，这时，1个CSS像素是等于4个物理像素的。

### 5 实现真正的1物理像素
当viewport的属性initial-scale为1时，页面大小正常，但initial-scale为0.5时，页面被缩小了1倍，像素比为2:1的设备本来1个CSS像素宽度占4个物理像素宽度，缩小后的1个CSS像素宽度就只占1个物理像素，即实现了真正的1物理像素。
eg：
border-width:1px并不是最小边框，浏览器可以显示的最小粒度比1px还要小。为什么会出现比border-width:1px更细的边框？
屏幕能够显示的最小粒度是1个物理像素，iPhone4的像素比为2，设置border-width:1px后，边框占了4个物理像素，如果能让边框的宽度为1物理像素，那么它就比1个CSS像素要细，这可以通过设置：
```
<meta name="viewport" content="width=device-width, initial-scale=0.5">
```

[<< 回到主页](http://suzy1993.github.io/misszy/)