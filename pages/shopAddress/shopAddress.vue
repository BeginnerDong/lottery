<template>
	<view class="">
		<view class="shopAddres">
			<view class="item flexRowBetween" v-for="(item,index) in shopAddres" :key="index" @click="seltCurr(index)">
				<view class="infor">
					<view class="flex">
						<image class="icon" src="../../static/images/stores-icon1.png" mode=""></image>
						<view class="title font14">中华世纪城智能柜机</view>
						<view class="phone color3 font13">15623565656</view>
					</view>
					<view class="adrs font13 color3">陕西省西安市雁塔区高新科技路西段239号中华世纪城</view>
				</view>
				<view class="arrow flexCenter">
					<image class="" :src="setCurr == index?'../../static/images/stores-icon2.png':'../../static/images/stores-icon3.png'"  mode=""></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				setCurr:0,
				shopAddres:[
					{},{},{}
				]
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '提货门店'
		},

		methods: {
			seltCurr(index){
				const self = this;
				self.setCurr = index
			},
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	
	page{background: #f5f5f5;padding-bottom: 140rpx;}
	.shopAddres .item{ padding: 30rpx 4%;background: #fff; margin-bottom: 10rpx;}
	.shopAddres .item .infor{ width: 88%;}
	.shopAddres .item .arrow{ width: 12%; justify-content: flex-end;}
	.shopAddres .item .icon{width: 30rpx; height: 30rpx; display: block; margin-right: 10rpx;}
	.shopAddres .item .arrow image{width: 40rpx; height: 40rpx; display: block;}
	.shopAddres .adrs{ margin-top: 16rpx; }
	.shopAddres .title{ margin: 0 20rpx 0 4rpx; }
	
</style>
