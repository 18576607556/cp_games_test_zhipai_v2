<template>
	<view class="">
		<u-modal v-model="show" title="真心话大冒险" :content="contentText"></u-modal>
		<!-- <view class="content_text" style="height: 50rpx;">
			{{contentText}}	
		</view> -->
		<view class="roulette">
			<image class="bg" src="@/static/bg_2.png" mode=""></image>
			<image :class="{start:isStart,roulette_circle:true}" src="@/static/roulette_circle.png" mode=""></image>
			<image class="line" src="@/static/roulette_circle_line.png" mode=""></image>
			<image class="play" src="@/static/roulette_play.png" @click="startZhuanpan" mode=""></image>
			<image class="select" src="@/static/roulette_select.png" mode=""></image>
			<view class="" style="text-align: center;">
				<image class="selected" src="@/static/roulette_selected.png" mode=""></image>
				<span class="select_text">{{contentText}}</span>
			</view>
			
		</view>
		</view>
</template>
<script>
	import zhuanpan_Data from '@/utils/data/zhuanpan_content.js'
	export default {
		data(){
			return{
					isStart:false,
					punishments: ['喝一杯','下家喝一杯','上家喝一杯','亲一个','鼻子碰鼻子','背后抱住对方','坐在对方腿上'],
					randomNum:0,
					show: false,
					contentText:'开始转转盘把!'
			}
					
			},
			onLoad(e){
				console.log(e,'传参') 
				let value = e.item? e.item.replace("\"([^\"]*)\"", ""):'base'; 				
				let res = zhuanpan_Data[value]
				if (!res){
					res = zhuanpan_Data['base']
				} 
				this.punishments=res
			},
			methods:{
				getRanDom(){
			 		
				},
				startZhuanpan(){
					if(this.isStart) return
					this.isStart=true
					let index=this.getRanDom()
					let startTime=Date.parse(new Date())
					//写个循环5s,
					let i=0
					let intervaFunc=setInterval(()=>{
						let endTime=Date.parse(new Date())
						if(endTime-startTime===3000||endTime-startTime>3000){					
							console.log()
							clearInterval(intervaFunc);
							this.isStart=false
							this.show = true;
						}
						if(i>this.punishments.length-1) i=0
						console.log(this.punishments[i],i)
						this.contentText=this.punishments[i]
						i++;
					},100)
					// 清空循环,设置值
					this.contentText=this.punishments[index]
				},
			}
			
	}
</script>
<style scoped>
	.roulette{
		position: relative;
		display: flex;
		justify-content: center;
		align-items: center;
		/* padding-top:80rpx; */
        /* background-image: linear-gradient(to right top, #79277e, #6d2571, #612265, #552059, #4a1d4d, #4a1d4d, #4a1d4d, #4a1d4d, #552059, #612265, #6d2571, #79277e);        */
	}
	.bg{
			height: 100vh;
			width:100vw;
	}
	
	
	.roulette_circle{
		position:absolute !important;
		top: 10rpx;
		width: 800rpx;
		height: 800rpx;
		/* animation: rotate 5s linear infinite; */
		/* transition: transform 10s ease-in-out; */
		z-index:17;
	}
	
	.start {
		 animation: rotate 3s ease-out infinite;
	}
	@keyframes rotate {
	  0% {
	    transform: rotate(0deg);
	  }
	  100% {
	    transform: rotate(360deg);
	  }
	}

	
	
	
	.line{
		position:absolute !important;		
		top: 10rpx;
		width: 800rpx;
		height: 800rpx;
		z-index:18;
	}
		
	.play{
		position:absolute !important;		
		top: 345rpx;
		left: 315rpx;
		width: 125rpx;		
		height: 125rpx;
		z-index:20;
	} 
	.select{
		position:absolute !important;
		top: 81rpx;
		left: 293rpx;
		width: 165rpx;		
		height: 350rpx;
		z-index:19;
	}
	.selected{
		position:absolute !important;
		top: -20rpx;
		left: 80rpx;
		width: 600rpx;		
		height: 150rpx;
		z-index:19;
	}
	.select_text{
		position:absolute !important;
		top: 5px;
		left: 175rpx;
		width: 420rpx;		
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		height: 150rpx;
		z-index:22;
	}
	</style>