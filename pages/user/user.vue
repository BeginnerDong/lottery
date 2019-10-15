<template>
	<view>
		<view class="userHead center radius10">
			<view class="phone">
				<image src="../../static/images/about-img.png" mode=""></image>
			</view>
			<view class="name">哆啦A梦</view>
			<view class="integral">积分：699</view>
		</view>
		
		<view class="userNav mglr4 flex boxShaow">
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_myEwm/user_myEwm'}})">
				<image class="icon" src="../../static/images/about-icon1.png" mode=""></image>
				<view>我的二维码</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_exchange/user_exchange'}})">
				<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
				<view>我的兑换</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_integral/user_integral'}})">
				<image class="icon" src="../../static/images/about-icon3.png" mode=""></image>
				<view>我的积分</view>
			</view>
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/user_Extension/user_Extension'}})">
				<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
				<view>我的推广</view>
			</view>
			<view class="item">
				<image class="icon" src="../../static/images/about-icon5.png" mode=""></image>
				<view>联系我们</view>
			</view>
		</view>
	</view>
</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onShow() {
			document.title = "我的"
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
			
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.orderGet(postData, callback);
			
			}
		}
	}
</script>

<style>
	
	page{background: #F5F5F5;}
	.userHead{ height: 298rpx; background:#f9524d url(../../static/images/about-icon.png) no-repeat 0 0/100% 100%; margin:20rpx 4% 40rpx 4%;padding: 40rpx;box-sizing: border-box; color: #FFF;border-radius: 16rpx;}
	.userHead .phone image{width:120rpx; height: 120rpx; border-radius: 50%;display: block;margin: 0 auto;}
	.userHead .name{ font-size: 30rpx; margin: 20rpx auto 10rpx auto;}
	.userHead .integral{ font-size: 28rpx;}
	.userNav{  display: flex; flex-wrap: wrap;padding: 20rpx 0; border-radius: 10rpx;background: #FFF;}
	.userNav .item{ width:33.3%; text-align: center; padding: 30rpx 20rpx; box-sizing: border-box; font-size: 26rpx; color: #666;}
	.userNav .item .icon{width: 50rpx; height: 50rpx; display: block; margin: 0 auto 20rpx auto;}
	
	
</style>
