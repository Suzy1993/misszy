[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS line-height属性

line-height: 200% | 2.0em | 2.0 的区别：
### 1 line-height: 200%
父元素的行高为200%时，会根据父元素的字体大小先计算出行高值然后再让子元素继承。所以当line-height:200%时，子元素的行高等于15px * 200% = 30px。
```
<div style="line-height:200%;font-size:15px;">
  父元素<div style="font-size:30px;">子元素</div>
</div>
```

### 2 line-height: 2.0em
父元素的行高为2.0em时，会根据父元素的字体大小先计算出行高值然后再让子元素继承。所以当line-height:2.0em时，子元素的行高等于15px * 2.0em = 30px。
```
<div style="line-height:2.0em;font-size:15px;">
  父元素<div style="font-size:30px;">子元素</div>
</div>
```

### 3 line-height: 2.0
父元素的行高为2.0时，会根据子元素的字体大小先计算出行高值然后再让子元素继承。所以当line-height:2.0时，子元素的行高等于30px * 2.0 = 60px。
```
<div style="line-height:2.0;font-size:15px;">
  父元素<div style="font-size:30px;">子元素</div>
</div>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
