写代码时经常会遇到这个问题，又总忘记怎么写😭，多余的话咱也不说了，直接看代码吧～

### 使用flex布局
```css
.father {
    display: flex;
    justify-content: center;
    align-items: center;
}
.son {
   ...
}
```
这种方式一看就懂，就不多说了。

### 绝对定位配合margin:auto
```css
.father {
    position: relative;
}
.son {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    margin: auto;
}
```
每个属性的作用可以看演示效果图：
<video src="./videos/absolute+margin.mov"></video>



### 绝对定位配合transform

```css
.father {
    position: relative;
}
.son {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```
注意子元素 top/left 属性的50%是相对父元素宽高的 50%，而 transform 属性在x、y轴平移的 -50% 是子元素宽高的 50%，有点儿绕，下面演示图一看便知：
<video src="./videos/absolute+transform.mov"></video>