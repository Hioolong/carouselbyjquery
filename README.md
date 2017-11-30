﻿# Jquery实现淡入淡出轮播图

思路
==

 - 用jquery实现轮播图的淡入淡出效果的话自然会想到jquery的fadeIn()和fadeOut(),还有的就是要将需要轮播的图片叠放在同一个盒子里面，为了实现流畅好看的淡入淡出效果
 - 制作轮播图的关键是通过定义一个全局索引变量index用于控制图片的显示
 - 因为html里后面加载的图片会将之前的图片覆盖掉，为了让图片按顺序显示，我给每张图片添加了递减的层级

---

实现
==

 

> HTML

```html
    <div class="box">
    <div class="ad">
        <ul class="img">
            <li><img src="images/1.jpg" alt="" ></li>
            <li><img src="images/2.jpg" alt="" ></li>
            <li><img src="images/3.jpg" alt="" ></li>
            <li><img src="images/4.jpg" alt="" ></li>
            <li><img src="images/5.jpg" alt="" ></li>
        </ul>
        <ol>
            <li class="current">1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
            <li>5</li>
        </ol>
    </div>
    <div class="arrow">
        <span class="left"><</span>
        <span class="right">></span>
    </div>
</div>
```

> CSS基本样式

```css
<style>
*{
    margin: 0;
    padding: 0;
    list-style: none;
}
.box{
    width: 992px;
    height: 420px;
    position: relative;
    border: 1px solid pink;
    margin: 100px auto;
}
.ad{
    width: 992px;
    height: 420px;
    position: relative;
}
.img img{
    width: 992px;
}
.ad ul li{
    width: 992px;
    height: 420px;
    position: absolute;
}
.ad ul li:first-child {
    position: absolute;
    z-index: 10;
}
.ad ul li:nth-child(2) {
    position: absolute;
    z-index: 9;
}
.ad ul li:nth-child(3) {
    position: absolute;
    z-index: 8;
}
.ad ul li:nth-child(4) {
    position: absolute;
    z-index: 7;
}
.ad ul li:nth-child(5) {
    position: absolute;
    z-index: 6;
}
.ad ul {
    position: absolute;
    left: 0;
    top: 0;
    width: 992px;
    height: 420px;
}
.ad ol {
    position: absolute;
    right: 10px;
    bottom: 10px;
    line-height: 20px;
    text-align: center;
    z-index: 11;
}
.ad ol li {
    float: left;
    width: 40px;
    height: 25px;
    background: #a9a9a8;
    border: 1px solid #ccc;
    margin-left: 10px;
    cursor: pointer;
    color: #fff;
}
 .ad ol li.current{
    background-color: #585858;
 }
.arrow {
    display: none;
}
.arrow span {
    width: 40px;
    height: 40px;
    line-height: 40px;
    position: absolute;
    top: 50%;
    margin-top: -20px;
    background-color: #000;
    cursor: pointer;
    text-align: center;
    font-weight: bold;
    font-family: '黑体';
    font-size: 30px;
    color: #fff;
    opacity: 0.3;
    border: 1px solid #fff;
    z-index: 11;
}
.arrow .left {
    left: 5px;
}
.arrow .right {
    right: 5px;
}
</style>
```

> jquery部分

    1. 首先要引入jq文件
```jquery
<script src="js/jquery-1.12.2.js"></script>
```
    2. jq代码
```jquery
<script>
        $(function(){
            //定义一个全局变量的索引
            var index = 0;
            //获取存放图片的li标签的数量
            var imgArrLength = $('.img li').length;
            //为右侧按钮注册点击事件
            $('.right').click(function() {
                if(index==imgArrLength-1){
                    index=-1;
                }
                index++;
                $('.img li').eq(index).fadeIn().siblings().fadeOut();
                $('.ad ol li').eq(index).addClass('current').siblings().removeClass('current');
                console.log(index);
            });
            //为左侧按钮注册点击事件
            $('.left').click(function(){
                if(index==0){
                    index=imgArrLength;
                }
                index--;
                $('.img li').eq(index).fadeIn().siblings().fadeOut();
                $('.ad ol li').eq(index).addClass('current').siblings().removeClass('current');
            })
            //为右下角的小按钮添加鼠标移入事件
            $('.ad ol li').mouseenter(function(){
                 $('.img li').eq($(this).index()).stop(true).fadeIn().siblings().fadeOut();
                 index = $(this).index();//将索引值设置为鼠标移入的小按钮的索引
                 //把鼠标移入的小方块的索引存给了全局变量Index之后再给改小方块添加current类
                 //让其高亮显示。
                 $('.ad ol li').eq(index).addClass('current').siblings().removeClass('current');
            })
            //定义了一个轮播图自动播放的函数
            function autoPlay(){
                if(index<imgArrLength-1){
                    index++; 
                    $('.img li').eq(index).fadeIn().siblings().fadeOut();
                    //让右下角小方块跟着图片一起动
                    $('.ad ol li').eq(index).addClass('current').siblings().removeClass('current');
                }
                if(index==imgArrLength-1){
                    index=-1;//把索引设置为-1，然后使定时器实现无缝轮播
                }
            }
            //定义了一个定时器
            var timeId = setInterval(autoPlay,1500);//定时器调用自动播放函数
            //鼠标移入图片盒子的时候清楚定时器，鼠标移出的时候再次开启定时器
            $('.box').hover(function() {
                clearInterval(timeId);
                $('.arrow').show();
            }, function() {
                timeId = setInterval(autoPlay,1500);
                $('.arrow').hide();
            });
        })
           
    </script>
```

总结
==
做轮播图的关键是在于控制全局索引index,通过控制index的边界值实现无缝轮播，处理逻辑都相对比较容易，实现起来还是比较容易的，要注意的就是全局图片索引index的变化。


    


