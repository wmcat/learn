# rem适配

## rem含义
> w3c的定义是相对于根元素的字体大小（font-size）在根元素的font-size属性中指定时，rem单位是指属性的初始值

这意味着1rem等于html元素的字体大小。

```
<meta name="viewport" content="width=device-width,user-scalable=no,initital-scale=1.0,
maximum-scale=1.0,minimum-scale=1.0">
<script>
 var pageWidth = window.innerWidth
 document.write('<style>html {font-size:' + pageWidth + 'px;}<stype>')
</script>
1rem = 1pagewidth

// 微调
pageWidth = pageWidth/10

// px 自动变成rem
scss
@function px2rem ($px) {
    @return $px/@designWidth * 10 + rem
}
```