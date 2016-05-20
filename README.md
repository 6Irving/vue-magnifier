# vue-magnifier

* 实现放大镜功能 


# Options(7个)

* :wrapX    外层包裹元素的坐标
* :wrapY    
* :boxWidth	  盒子大小(包裹 原图和放大图 的盒子)
* :boxHeight  
* :gap    两个盒子的间距
* :scale      缩放比
* :imgSrc      图片地址


# test.vue

```
<template>
	<magnifier 
        :wrap-x='wrapX' 
        :wrap-y='wrapY' 
        :box-width='boxWidth' 
        :box-height='boxHeight' 
        :scale='scale' 
        :gap='gap' 
        :img-src='imgSrc'>
    </magnifier>
</template>

<script>
    import magnifier from './magnifier';
    export default {
        data(){
            wrapX: 100,
            wrapY: 100,
            boxWidth: 300,
            boxHeight: 225,
            scale: 2,
            imgSrc: 'http://img2.3lian.com/2014/f5/158/d/86.jpg',
            gap: 100
        },
        components:{
            magnifier
        }
    }
</script>
```
