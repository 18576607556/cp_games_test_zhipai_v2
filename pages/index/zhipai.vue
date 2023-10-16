<template>
	<view class="bg">
		<u-modal v-model="show" title="真心话大冒险" :content="content"></u-modal>
		<view class="sz"></view>
		<view class="htitle">
			<view @click="changeMan(1)" :class="{'man': true, 'manActive': flag === 1}">
				<image class="man-png" src="../../static/man.png" mode="widthFix"></image>
			</view>
			<view @click="changeMan(0)"   :class="{'man': true, 'manActive': flag === 0}">
				<image class="man-png" src="../../static/woman.png"></image>
			</view>
		</view>
		<view :style="{marginLeft: left+'px'}" class="blocks">
			<view @click="clickBlock(item)" :class="{'vis':item.hasClick, 
			'block': true, 'center-block': item.center, 'start':isStart && item.center,
			 'huohua':huohua && item.center, 'dis':huohua && !item.center}"  
			 v-for="(item, index) in blocks">
				<view v-if="huohua && item.center" class="fire"></view>
			</view>
		</view> 
		
		<view class="guize">
			<rich-text :nodes="guizenneirong"></rich-text>
		</view>
	</view>
</template>
 
<script>
import Tab from '@/utils/tab.js'
import zhuanpan_Data from '@/utils/data/zhipai_content.js'
	export default {
		data() {
			return {
				blocks: [{hasClick: false}, {hasClick: false}],
				left: '9',
				flag: 1,
				show: false,
				content: '',
				isStart:false,
				huohua: false,
				punishments: {man: [], woman: []},
				guizenneirong: `
					1. 注意事项<br>
					   建议播放紧张氛围的音乐<br>
				`
			}
		},
		onLoad(e) {
			const systemInfo = uni.getSystemInfoSync();
			let width = systemInfo.windowWidth < 700 ? systemInfo.windowWidth : 700;
			let blockWidth = 64;
			this.left  = width%blockWidth /2 ;
			// 计算列数
			let colNums = Number.parseInt(width/blockWidth);
			console.log('colNums', colNums)
			let num = 48; 
			// 计算行数量 
			let rowNums =  Math.ceil(num/colNums);
			console.log('rowNums', rowNums);
			
			// 计算中间行
			let centerRows =  Math.ceil(rowNums/2);
			console.log('centerRows', centerRows);
			// 计算中间列
			let centerCols =  Math.ceil(colNums/2);
			console.log('centerCols', centerCols);
			
			// 找到最中间的I值
			let centerI = (centerRows - 1) * colNums + centerCols;
			
			let dis = [centerI-1, centerI+1, centerI-colNums, centerI+colNums]
			dis = [centerI+1, centerI+colNums, centerI+colNums+1]
			
			// 要容纳半个屏幕
			// 随机生成一百个blocks
			let arrs = [];
			for (var i = 0; i < num; i++) {
				if (i === centerI - 1){
					arrs.push({index: i, center: true})
					continue;
				}
				if (dis.findIndex(item => item - 1 === i) > -1){
					arrs.push({index: i, hasClick: true, dis: true})
					continue;
				}
				arrs.push({index: i, hasClick: false})
			}		
			this.blocks = arrs;
			
			if (!e || !e.item){
				this.punishments=zhuanpan_Data['juchan']
			} else {
				let value = e.item;
				let res = zhuanpan_Data[value]
				if (!res){
					res = zhuanpan_Data['juchan']
				} 
				this.punishments=res
			} 
			//根据路由传参配置列
			// this.punishments=zhuanpan_Data['']
			// Object.keys(Mode).forEach(key=>{
			// 	if(value.indexOf(Mode[key]) > -1){
			// 		this.punishments=zhuanpan_Data[key]
			// 	}
			// })
		},
		methods: {
			changeMan(flag) {
				this.flag = flag;
			},
			showTips(obj){
				obj.hasClick = true;
				
				if (Math.floor((Math.random() * 10))%3 === 0){
					// 随机获取内容
					this.show = true;
					const arrs = this.punishments[this.flag===1? "man":"woman"];
					console.log('this.punishments[this.flag===1? "man":"woman"]', arrs[Math.floor(Math.random() * arrs.length)])
					this.content = arrs[Math.floor(Math.random() * arrs.length)];
				}
				
				
				// 给中间按钮增加齿轮
				this.huohua = true;
				
				if (this.blocks.findIndex(block => !block.hasClick && !block.center) === -1){
					this.blocks.forEach(block => {
						if (block.dis || block.center){
							return;
						}
						block.hasClick = false;
					})
				}
			},
			clickBlock(obj) { 
				console.log('obj', obj)
				if (obj.center || obj.dis){
					if (this.isStart){
						return;//防止连续点击
					}
					this.huohua = false;
					this.isStart=true
					console.log('start')
					this.$nextTick(() => {
						let intervaFunc=setTimeout(()=>{
							this.isStart=false;
							this.changeMan(Math.floor(Math.random() * 2));
						},1000)
					})
					return;
				}
				this.showTips(obj);
			}
		}
	}
</script>

<style lang="scss" scoped >
	.dis{
		// background-color: #0000004a !important;
		opacity: 0.8;
	} 
	.fire {
		left: 50%; 
		top: 50%;
		width: 0;
		height: 0;
		position: relative;
		border-style: solid;
		border-width: 0 12.5% 20% 12.5%;
		border-color: transparent transparent #f00 transparent;
		border-radius: 0 0 20px 20px;
		box-shadow: 0 0 10px #f00;
		animation: burn 1s infinite;
	}
	@keyframes burn {
		0% {
		transform:scale(2.0);
		opacity: 0.5;
		}
		
		50% {
		transform:scale(1.8);
		opacity: 1;
		}
		100% {
		transform:scale(2.0);
		opacity: 0.5;
		}
	}
	
	.huohua {
		border-radius: #fff;
		animation: burn 1s infinite;
	}
	
	.start {
		 transform:scale(1.5);
		 animation: rotate 1s ease-out infinite;
		 animation-iteration-count: 1;
	}
	@keyframes rotate {
	  0% {
	    transform: rotate(0deg);
	  }
	  100% {
	    transform: rotate(360deg);
	  }
	}
	
	.guize{
		padding: 10px;
		font-size: 22px;
		font-weight: bold;
		color: #fddbff;
	}
	.htitle{
		height: 55px; 
		width: 200px;
		border: solid 3px #bb96d1;
		margin: auto;
		border-radius: 7px;
		display: flex;
		
		.manActive{
			border-right: solid 3px #d4aaed;
			background-color: #d4aaed;
		}
	}
	.man{
		width: 100px;
		height: 50px; 
		background-color: #0000004a;
		text-align: center;
		.man-png{
			width: 50px;
			height: 50px; 
		}
	}
	.bg {
		
		// .sz{
		// 	width: 100px;
		// 	height: 100px;
		// 	position: absolute;
		// 	background-color: #fff;
		// 	top: 50%;
		// 	left: 50%;
		// }
		height: calc(100%);
		width: calc(100%);
		padding-top: 20px;
	}
	.vis{
		opacity: 0;
	}
	.blocks{
		// margin: auto;
		// width: 400px;
		margin-top: 20px;
		display: flex;
		flex-wrap: wrap;
		// gap:18px;
		
		.center-block{
			background-color: #ec79ef !important;
			background-image: url('../../static/shaizi.png');
			background-size: cover;
			position: relative;
			left: 30px;
			top: 30px;
			transform:scale(1.5);
		}
		.block{
			background-color: #bb96d1;
			height: 50px;
			width: 50px;
			border-radius: 10px;
			margin: 7px;
		}
	}
</style>
