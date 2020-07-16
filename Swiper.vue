<template>
	<!-- 轮播 -->
	<div id="banner" >
		<div class="container" 
		:style="{transition:slideTime+'s',marginLeft:-screen*num+'px'}">
			<!-- 轮播图片所在容器 -->
				<img v-for="(item,index) in bannerImg" 
				:src="item.img" 
				:key="index">
			<!-- 指示点 -->
		</div>
			<div class="buttons">
				<div class="button" 
				v-for="(item,index) in bannerImg" 
				:key="index" 
				:class="{activeButton:currentBtn(index)}" 
				@click="bthClick(index)">
					
				</div>
			</div>
		
	</div>
</template>

<script>
	export default {
		name: "Banner",
		data() {
			return {
				bannerImg:[
					{
						id:0,
						img:'https://s10.mogucdn.com/mlcdn/c45406/180926_45fkj8ifdj4l824l42dgf9hd0h495_750x390.jpg'
					},
					{
						id:1,
						img:'https://s10.mogucdn.com/mlcdn/c45406/180926_31eb9h75jc217k7iej24i2dd0jba3_750x390.jpg'
					},
					{
						id:2,
						img:'https://s10.mogucdn.com/mlcdn/c45406/180919_3f62ijgkj656k2lj03dh0di4iflea_750x390.jpg'
					}
					],
				button:0,//轮播按钮指定点个数，根据轮播图数量获取
				slideTime:1,//滑动时间
				num:0,//轮播图个数
				timer:null,//定时器
				screen:null,//屏幕宽度
				reTime:3000 ,//图片停留时间
				currentIndex:null//图片的下标
			}
		},
		methods:{
			// 判断当前图片的index是否和图片个数相等,相等返回true,绑定的动态样式生效
			currentBtn(index){
				if(index == this.num){
					return true
				}else {
					return false
				}
			},
			// 设置点击按钮切换到对应的轮播图
			bthClick(index){
				this.num = index 
				this.slideTime = 0 //点击切换时清除滑动时间
			},
			// 图片轮播
			get(){
				// 获取屏幕宽度值给screen
				this.screen = document.body.clientWidth;
				// 当图片滑动=3时,将滑动时间置0,将图片数置为0,滑动时间设置为0视觉不会出现倒退
				if(this.num == this.bannerImg.length-1){
					this.slideTime = 0
					this.num = 0
				}else{
					// 上面条件不满足时滑动时间为1,图片个数每次递增1
					this.slideTime = 1
					this.num++
				}
			}
		},
		// 页面创建完成后执行该函数
		mounted(){
			this.timer = setInterval(this.get,this.reTime)
		}
	}
</script>

<style>
	/* 外层视图,只显示一张图片的大小 */
	#banner{
		width: 100%;
		position: relative;
		overflow: hidden;
	}
	/* 轮播图容器,宽度为视图的6倍 */
	.container{
		width: 600%;
		transition: 2s;
	}

	.container img{
		width: 16.7%;
		height: 320px;
	}
	
	.buttons{
		position: absolute;
		left: 50%;
		top: 90%;
		transform: translate(-50%, -50%);
		display: flex;
	}
	.button{
		display: inline-block;
		margin-left: 20px;
		width: 6px;
		height: 6px;
		border-radius: 4px;
		background-color: white;
	}
	.activeButton{
		background-color: #f00;
	}
</style>
