<template>
	<view class="tabarIndex">
		<u-toast ref="uToast" />
		<u-modal v-model="show"   @confirm="confirm"  :mask-close-able="true" title="识别码认证">
			<view class="slot-content">
				<view class="_title">关注公众号，获取识别码</view>
				<view class="_width" style="width: 85%; margin: auto;">
					<u-input v-model="code" width="80%" placeholder="请输入识别码" :focus="true" :border="true" />
				</view>
			</view>
		</u-modal>
				
		<view v-if="selectTab === 0">
				<component  class="cbody" is="index"></component>
		</view>
		
		<view v-show="selectTab !== 0" >
			<view class="header">
				{{title}}
			</view>
			<view class="content">
				<view class="content_item" v-for="(item,index) in modeList" :key="index+'cont'" @click="toDetail(item, index)">
					{{item.value}} <text class="go_btn">Go ></text>
				</view>
			</view>
		</view>
		<view class="tabarFooter">
			<view v-for="(item ,index) in tabList" :key="index+'tab'" @click="handleTab(index)"
				:class="index===selectTab?'tabitem  select_tab':'tabitem'">
				<!-- <image  class="icon" :src="item.img" mode=""></image> -->
				{{item.value}}
			</view>
		</view>
	</view>
</template>

<script>
	import Index from './subpage/index'
	import tabData from '@/utils/tab.js'
	export default {
		components: {
					Index
				},
		data() {
			return {
				title: '情侣飞行棋',
				url: '',
				code: '',
				tabList: [],
				show: false,
				modeList: [],
				selectTab: 0,
				codes: {
					'135677': true
				}
			}
		},
		onLoad(){
			this.tabList=tabData
			Mode.forEach(item=>{
				if(tabData[this.selectTab].gameName===item.gameName) this.modeList=item.modeList
			})
		},
		mounted() {
			const hostnames = ['x', 'y', 'a', 's', 'k'] 
		},
		methods: {
			confirm() {
				if (this.code !== '' && this.codes[this.code.trim()]){
					document.cookie = this.code;
					
					uni.navigateTo({
					  url: this.url
					})
				} else {
					this.$refs.uToast.show({
						title: '错误的认证码， 请检查授权码是否正确。',
						type: 'error'
					})
					this.show = ture;
				}
			},
			auth(url) {
				
				if (!document.cookie || !this.codes[document.cookie]){
					// 弹窗， 需要认证
					console.log('999')
					this.url = url;
					this.show = true;
					return;
				}
				
				uni.navigateTo({
				  url: url
				})
			},
			toDetail(mode, index){
				const page = tabData[this.selectTab];
				if (!page.url){
					return;
				}
				const url = page.url +`?item=${mode.keyName}`;
				if (index > 0){
					// 需要输入密码框
					this.auth(url);
				} else {
					uni.navigateTo({
					  url: url
					})
				}
			},
			handleTab(index) {
				this.selectTab = index
				this.modeList = tabData[index].modeList
			},
			changeTabar(index) {},
		}
	}
</script>

<style lang="scss" scoped>
	._title{
		margin: 5px;
		font-size: 18px;
		text-align: center;
		font-weight: bold;
		// color: #fff;
	}
	body::-webkit-scrollbar{
			display: none;
		}
		
	.tabarIndex {
		background-color: #1f1f29;
		height: 100vh;

		.content {
			display: flex;
			flex-direction: column;
			align-items: center;
			padding: 30rpx;

			&_item {
				margin-bottom: 30rpx;
				width: 80%;
				height: 150rpx;
				border-radius: 10rpx;
				background-color: #e4f0ff;
				text-align: center;
				line-height: 150rpx;
				font-size: 40rpx;
				color: #41a1f1;

				.go_btn {
					height: 30rpx;
					width: 80rpx;
					color: #fff;
					background-color: #41a1f1;
					border-radius: 30rpx;
					font-size: 12rpx;
					padding: 4rpx 8rpx;
					line-height: 30rpx;
					margin-left: 10rpx
				}

			}

			&_item:nth-child(2) {
				color: #fdb24c;
				background-color: #fff4e1;

				.go_btn {
					background-color: #fdb24c;
				}
			}

			&_item:nth-child(3) {
				background-color: #ffebf0;
				color: #eb83a1;

				.go_btn {
					background-color: #eb83a1;
				}
			}

			&_item:nth-child(4) {
				background-color: #5765b4;
				color: red;
				.go_btn {
					background-color: red;
				}
			}

			&_item:nth-child(5) {
				background-color: #27ab60;
				color: #5765b4;

				.go_btn {
					background-color: #5765b4;
				}
			}
		}

		.tabarFooter {
			position: fixed;
			bottom: 0;
			left: 0;
			right: 0;
			display: flex;
			height: 100rpx;
			background-color: #fff;
			z-index: 100;
			.tabitem {
				width: 33%;
				display: flex;
				flex-direction: column;
				align-items: center;
				justify-content: center;

				.icon {
					width: 80rpx;
					height: 80rpx;
				}
			}

			.select_tab {
				color: #f4c45d;
			}
		}
	}
</style>