<template>
	<view>
		<view class="myEwm">
			<image class="pic" src="../../static/images/qr_code-img1.png" mode=""></image>
			<view class="ewm">
				<image src="../../static/images/myEwm.png" mode=""></image>
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
			document.title = "我的二维码"
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
.myEwm{position: fixed; top: 0;right: 0; bottom: 0;left: 0;}
.myEwm .pic{width: 100%; height: 100%; display: block;}
.myEwm .ewm{ width: 300rpx; height: 300rpx; display: block; position: absolute; bottom: 160rpx;left: 50%;transform: translateX(-50%); z-index: 10;}
.myEwm .ewm image{width: 100%; height: 100%; display: block;}
</style>
