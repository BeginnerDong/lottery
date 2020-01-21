<template>
	<view>
		<view class="myEwm">
			<image class="pic" src="../../static/images/qr_code-img1.png" mode=""></image>
			<view class="ewm">
				<image :src="mainData.url" mode=""></image>
			</view>
		</view>
		
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
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.param =  uni.getStorageSync('user_info').user_no,
				postData.ext = 'png';
				const callback = (res) => {
					console.log(res);
					self.mainData = res.info;
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.getQrCommonCode(postData, callback);
			},
		}
	}
</script>

<style>
.myEwm{position: fixed; top: 0;right: 0; bottom: 0;left: 0;}
.myEwm .pic{width: 100%; height: 100%; display: block;}
.myEwm .ewm{ width: 300rpx; height: 300rpx; display: block; position: absolute; bottom: 160rpx;left: 50%;transform: translateX(-50%); z-index: 10;}
.myEwm .ewm image{width: 100%; height: 100%; display: block;}
</style>
