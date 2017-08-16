[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS3 新特性——animation

### 1 创建动画
@keyframes规则——指定一个CSS样式，动画将逐步更改样式。可以改变任意多的样式任意多的次数。  
用百分比或用关键词"from" 和 "to"来规定变化发生的时间。"from" 和 "to"分别等同于 %和100%，0%是动画的开始，100%是动画的完成。
```
@keyframes myAnimation{
    0%{background:red;}
    25%{background:orange;}
    50%{background:yellow;}
    75%{background:green;}
    100%{background:blue;}
}
```
注意 -webkit-, -ms- 或 -moz- 前缀。

### 2 动画绑定到选择器
animation属性——所有动画属性的简写属性，除了animation-play-state属性。  
指定至少两个子属性：动画名称animation-name、动画时长animation-duration。
```
div{
    animation:myAnimation 20s;
}
```
注意 -webkit-, -ms- 或 -moz- 前缀。

### 3 动画静置
可以将动画细分为动画和静置两个阶段。
```
@keyframes myAnimation{
    0%,20%{background:red;}
    30%,40%{background:orange;}
    50%,60%{background:yellow;}
    70%,80%{background:green;}
    90%,100%{background:blue;}
}
```
如40%~60%包含：40%~50%的动画阶段和50%~60%的静置阶段。

### 4 animation属性
#### 4.1 animation-name属性
为@keyframes动画指定名称。
* animation-name:none——指定没有动画,可用于覆盖从级联的动画。

#### 4.2 animation-duration属性
定义动画完成一个周期需要的秒或毫秒数。默认值为0，意味着没有动画效果。

#### 4.3 animation-timing-function属性
指定动画将如何完成一个周期。定义动画从一套CSS样式变为另一套CSS样式所用的时间，用于使变化更为平滑。
* animation-timing-function:linear——指定动画从头到尾的速度相同。
* animation-timing-function:ease——指定动画以低速开始，然后加快，在结束前变慢，默认值。
* animation-timing-function:ease-in——指定动画以低速开始。
* animation-timing-function:ease-out——指定动画以低速结束。
* animation-timing-function:ease-in-out——指定动画以低速开始和结束。
* animation-timing-function:cubic-bexier(n,n,n,n)——自定义。cubic-bezier为贝兹曲线中的绘制方法。四点P0-P3中，P0、P3是默认的点，对应了[0,0]、[1,1]，剩下的P1、P2两点则是通过cubic-bezier(x1, y1, x2, y2) 自定义的，x1、x2、y1、y2的值范围在[0, 1]。
    * ease: cubic-bezier(0.25, 0.1, 0.25, 1.0)
    * linear: cubic-bezier(0.0, 0.0, 1.0, 1.0)
    * ease-in: cubic-bezier(0.42, 0, 1.0, 1.0)
    * ease-out: cubic-bezier(0, 0, 0.58, 1.0)
    * ease-in-out: cubic-bezier(0.42, 0, 0.58, 1.0)

#### 4.4 animation-delay属性
定义动画什么时候开始，单位可以是秒（s）或毫秒（ms）， 默认值为0，允许负值，如-2s 使动画马上开始，但跳过 2 秒进入动画。

#### 4.5 animation-iteration-count属性
定义动画应该播放多少次。
* animation-iteration-count :n——定义动画应该播放n次。
* animation-iteration-count :infinite——定义动画应该播放无限次。

#### 4.6 animation-direction属性
定义是否循环交替反向播放动画。如果动画被设置为只播放一次，该属性将不起作用。
* animation-direction:normal——默认值，动画正常播放。
* animation-direction:reverse——动画反向播放。
* animation-direction:alternate——动画在奇数次（1、3、5...）正向播放，在偶数次（2、4、6...）反向播放。
* animation-direction:alternate-reverse——动画在奇数次（1、3、5...）反向播放，在偶数次（2、4、6...）正向播放。

#### 4.7 animation-fill-mode属性
规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。
默认情况下，CSS 动画在第一个关键帧播放完之前不会影响元素，在最后一个关键帧完成后停止影响元素。animation-fill-mode属性可重写该行为。
* animation-fill-mode:none——默认值。不设置结束之后的状态，默认情况下回到跟初始状态一样。
* animation-fill-mode:forwards——动画元素设置为整个动画结束时的状态。
* animation-fill-mode:backwords——设置动画结束之后回到初始状态。
* animation-fill-mode:both——设置为结束或者开始时候的状态。一般都是回到默认状态。

#### 4.8 animation-play-state属性
指定动画是否正在运行或已暂停。可以在JavaScript中使用此属性在一个周期中暂停动画。
* animation-play-state:paused——指定暂停动画。
* animation-play-state:running——指定正在运行的动画。

[<< 回到主页](http://suzy1993.github.io/misszy/)
