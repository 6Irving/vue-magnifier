<template>
	<div id="magnifier" :style="{'left':wrapX + 'px','top':wrapY + 'px'}">
		<div id="smallBox" :style="{'width':boxWidth + 'px','height':boxHeight + 'px'}">
			<div id="mark" @mouseover="mouseOver" @mouseleave="mouseLeave" @mousemove="mouseMove"></div>
			<div id="floatBox" :style.sync="{'left':floatBoxLeft + 'px','top':floatBoxTop + 'px','width':floatBoxWidth + 'px','height':floatBoxHeight + 'px'}" v-show='show'></div>
			<img id="smallImg" :style="{'width':boxWidth + 'px','height':boxHeight + 'px'}" :src="imgSrc">
		</div>
		<div id="bigBox" :style="{'width':boxWidth + 'px','height':boxHeight + 'px','left':bigBoxLeft+'px'}"  v-show='show' transition="magnifier" transition-mode="out-in">
			<img id="bigImg" :style.sync="{'left':bigImgLeft + 'px','top':bigImgTop + 'px','width':bigImgWidth + 'px','height':bigImgHeight + 'px'}" :src="imgSrc">
		</div>
	</div>
</template>
<script>
	export default {
		//父组件传进的参数(7个：外层包裹元素的坐标、盒子大小和间距、缩放比、图片地址)
		props: {
			// 外层包裹元素的坐标
			wrapX: {
				type: Number,
				default: 0,
			},
			wrapY: {
				type: Number,
				default: 0,
			},
			// 两个盒子的大小(包裹 原图和放大图 的盒子)
			boxWidth: {
				type: Number,
				default: 0,
			},
			boxHeight: {
				type: Number,
				default: 0,
			},
			// 缩放比例: 浮动盒子和大图相对于 两个盒子的 缩放比
			scale: {
				type: Number,
				default: 1, 
				validator: function(value){
					return value > 0;
				}
			},
			// 图片地址
			imgSrc: {
				type: String,
				default: '',
			},
			// 两个盒子的间距
			gap: {
				type: Number,
				default: 0,
			}
		},
		data() {
			return {
				show: false,     //控制浮动盒子和大图的显示
				floatBoxLeft: 0,  //浮动盒子的坐标(随鼠标移动)
				floatBoxTop: 0,
				bigImgLeft: 0,    //放大图的坐标(随鼠标移动)
				bigImgTop: 0
			}
		},
		// 计算属性(通过父组件传入的属性计算的结果,5个计算属性：浮动盒子的大小、放大盒子的left、放大图的大小)
		computed: {
			floatBoxWidth(){
				return this.boxWidth/this.scale;
			},
			floatBoxHeight(){
				return this.boxHeight/this.scale;
			},
			bigBoxLeft(){
				return this.boxWidth+this.gap;
			},
			bigImgWidth(){
				return this.boxWidth*this.scale;
			},
			bigImgHeigth(){
				return this.boxHeight*this.scale;
			}
		},
		methods: {
			// 鼠标移入、移出、移动事件,绑定在同一个元素#mark上
			
			// mouseover事件,不是mouseenter
			mouseOver: function(){
				this.show = true;
			},
			mouseLeave: function(){
				this.show = false;
			},
			mouseMove: function(ev){
				let that = this;
				ev = ev || window.event;
				let floatBox_left = ev.clientX - that.wrapX - that.floatBoxWidth/2;
				let floatBox_top = ev.clientY - that.wrapY - that.floatBoxHeight/2;

				// 处理鼠标移动到边界情况
				if(floatBox_left < 0){
					floatBox_left = 0; 
				}else if(floatBox_left > that.boxWidth - that.floatBoxWidth){
					floatBox_left = (that.boxWidth - that.floatBoxWidth);
				}

				if(floatBox_top < 0){
					floatBox_top = 0; 
				}else if(floatBox_top > that.boxHeight - that.floatBoxHeight){
					floatBox_top = that.boxHeight - that.floatBoxHeight;
				}

				// 浮动盒子的坐标
				that.floatBoxLeft = floatBox_left ;
				that.floatBoxTop = floatBox_top;
				// 放大图的坐标
				that.bigImgLeft = -that.scale * floatBox_left;
				that.bigImgTop = -that.scale * floatBox_top;
			}
		}
	}
</script>

<style>
	*{
		margin: 0;
		padding: 0;
	}
	#magnifier,#mark, #floatBox, #smallImg,#smallBox,#bigBox,#bigImg{
		position: absolute;
		z-index: 10000;
	}
	#smallBox,#mark{
		width: 100%;
		height: 100%;
		left: 0;
		top: 0;
		z-index: 10005;
	}
	#floatBox{
		background-color: #fff;
		opacity: 0.5;
		z-index: 10003;
	}
	#smallImg{
		left: 0;
		top: 0;
		z-index: 10001;
	}
	#bigBox{
		top: 0px;
		overflow: hidden;
	}
	.magnifier-transition {
	    border: 1px solid #DEDEDE;
	    border-radius: 2px;
	    transition: all .5s ease;
	}
	.magnifier-enter, .magnifier-leave {
	    opacity: 0;
	    height: 0;
	}
</style>