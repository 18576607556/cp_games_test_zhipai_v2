<template>
	<view class="sex-block flex-column">
		<view class="remark-li">
			可选择所在城市，更精准的结识近距离好友！
		</view>
		<view class="li flex-row-left">
			<u-icon name="map-fill" color="#000000"></u-icon>
			<input style="font-size: 25rpx; margin-left: 20rpx;" type="password" value="" v-model="code" placeholder="请输入活动码."/>
		</view>
		<view class="li flex-row-left">
			<u-icon name="tags-fill" color="#000000"></u-icon>
			<input style="font-size: 25rpx; margin-left: 20rpx;" type="text" value="" v-model="paperContent" placeholder="请输入纸条内容"/>
		</view>
		<view class="btn flex-row-center" @click="toPayFree()">
			立即投入纸条
		</view>
		<view class="remark flex-row-center">
			投的越多，被抽到的概率越大！
		</view>
		<view class="back-btn flex-row">
			<view @click="$emit('changenav', 0)">返回上一步</view>
		</view>
		<u-picker mode="region" v-model="addrShow" :area-code='initAddr' @confirm="getAddrCode"></u-picker>
		
	</view>
</template>

<script>
	// const db = uniCloud.database();
	// const uid = db.getCloudEnv('$cloudEnv_uid');
	export default {
		props: {
			sex: {
				type: Number,
				default: 1
			}
		},
		data() {
			return {
				code: '',
				form1: {
				  openid: ""
				},
				out_trade_no:'',
				alipayAppPayToH5Pay:false,
				noClick: true,
				sexIndex: 1,
				addrShow: false,
				addrName: '',
				addrCode: '',
				initAddr: [],
				paperContent: '',
			}
		},
		mounted() {
			this.sexIndex = this.sex
		},
		methods: {
			getAddrCode(addr) {
				var addrName = addr.province.label + addr.city.label + addr.area.label
				this.addrName = addrName
				this.addrCode = addr.area.value
				this.initAddr = [addr.province.value, addr.city.value, addr.area.value]
			},
			
			
			async toPayFree() {
				if (this.code != '894562'){
					this.$showToast('活动码不正确， 获取方式请看公众号。')
					return;
				}
				uni.showLoading({
					title: '投入中...',
					mask: true
				});
				try{
					// await db.collection('paper').add({
					// 	address:this.addrName,
					// 	addcode:this.addrCode,
					// 	status:this.sexIndex,
					// 	wx:this.paperContent
					// })
					uni.hideLoading();
					this.$showToast('投入成功')
					this.noClick = true;
				}catch(e){
					this.$showToast(e.message)
					uni.hideLoading();
					this.noClick = true;
				}
			},
			putMsg(){
				this.$emit("putmsg", {
					navIndex: 3,
					sexIndex: me.sexIndex
				})
			},
		}
	}
</script>

<style lang="scss" scoped>
	.sex-block {
		padding: 30rpx 0rpx;
		padding-top: 140rpx;
		.remark-li {
			color: $minor-text-color;
			font-size: 24rpx;
			margin-bottom: 10rpx;
		}

		.li {
			width: 90%;
			background-color: #f2edfc;
			border-radius: 200rpx;
			padding: 30rpx;
			margin-top: 30rpx;
			.text1 {
				width: 90%;
				margin-left: 20rpx;
				font-size: 25rpx;
				color: $minor-text-color;
			}
			.text2 {
				font-size: 25rpx;
				width: 90%;
			}
		}

		.btn {
			width: 90%;
			margin-top: 50rpx;
			background-color: #FFD800;
			padding: 30rpx;
			border-radius: 200rpx;
			font-weight: bold;
		}

		.remark {
			color: $minor-text-color;
			font-size: 24rpx;
			margin-top: 20rpx;
		}

		.back-btn {
			width: 100%;
			color: $minor-text-color;
			font-size: 24rpx;
			margin-top: 30rpx;
			text-decoration: underline;
			justify-content: space-around;
			.right{
				color: $main-text-color;
			}
		}
	}
</style>
