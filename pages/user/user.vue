<template>
	<view>
		<form @submit="formIdAdd" report-submit="true">
		<view class="userHead center radius10">
			<view class="phone">
				<open-data type="userAvatarUrl"></open-data>
			</view>
			<view class="name"><open-data type="userNickName"></open-data></view>
			<view class="integral">积分：{{mainData.info?mainData.info.score:''}}</view>
		</view>
		
		<view class="userNav mglr4 flex boxShaow">
			<button class="item" form-type="submit" @click="Router.navigateTo({route:{path:'/pages/user_myEwm/user_myEwm'}})">
				<image class="icon" src="../../static/images/about-icon1.png" mode=""></image>
				<view>我的二维码</view>
			</button>
			<button class="item" form-type="submit" @click="Router.navigateTo({route:{path:'/pages/user_exchange/user_exchange'}})">
				<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
				<view>我的兑换</view>
			</button>
			<button class="item" form-type="submit" @click="Router.navigateTo({route:{path:'/pages/user_integral/user_integral'}})">
				<image class="icon" src="../../static/images/about-icon3.png" mode=""></image>
				<view>我的积分</view>
			</button>
			<button class="item" form-type="submit"  @click="Router.navigateTo({route:{path:'/pages/user_Extension/user_Extension'}})">
				<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
				<view>我的推广</view>
			</button>
			<button class="item" form-type="submit" @click="phoneCall()">
				<image class="icon" src="../../static/images/about-icon5.png" mode=""></image>
				<view>联系我们</view>
			</button>
		</view>
		</form>
	</view>
</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getMainData','getPhoneData'], self);
		},
		
		methods: {
			
			
			
			formIdAdd(e) {
				const self = this;
				console.log(e)
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
			
			phoneCall() {
				const self = this;
				wx.makePhoneCall({
					phoneNumber: self.phoneData.description,
				})
			},
			
			getPhoneData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					title:'客服'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.phoneData = res.info.data[0];
					}
					console.log('self.phoneData', self.phoneData)
					self.$Utils.finishFunc('getPhoneData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userGet(postData, callback);
			},
			
		},
	};
</script>

<style>
	
	page{background: #F5F5F5;}
	button{
		background: none;
		line-height: 1.5;
	}
	button::after{
		border: none;
	}
	.button-hover{
		color: #000000;
		background: none;
	}
	.userHead{ height: 298rpx; background:#f9524d url(../../static/images/about-icon.png) no-repeat 0 0/100% 100%; margin:20rpx 4% 40rpx 4%;padding: 40rpx;box-sizing: border-box; color: #FFF;border-radius: 16rpx;}
	.userHead .phone{width:120rpx; height: 120rpx; border-radius: 50%;display: block;margin: 0 auto;overflow: hidden;}
	.userHead .name{ font-size: 30rpx; margin: 20rpx auto 10rpx auto;}
	.userHead .integral{ font-size: 28rpx;}
	.userNav{  display: flex; flex-wrap: wrap;padding: 20rpx 0; border-radius: 10rpx;background: #FFF;}
	.userNav .item{ width:33.3%; text-align: center; padding: 30rpx 20rpx; box-sizing: border-box; font-size: 26rpx; color: #666;}
	.userNav .item .icon{width: 50rpx; height: 50rpx; display: block; margin: 0 auto 20rpx auto;}
	
	
</style>
