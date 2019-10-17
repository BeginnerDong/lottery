<template>
	<view>
		<view>
			<image style="width: 100%; height: 346rpx; display: block;padding-bottom: 60rpx;" src="../../static/images/registered-img.png" mode=""></image>
		</view>
		<view class="pub_editInfor">
			<view class="item">
				<view class="name">用户名：</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入用户名或手机号"/>
				</view>
			</view>
			<view class="item">
				<view class="name">验证码：</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入用户名或手机号"/>
				</view>
			</view>
			<view class="item">
				<view class="name"></view>
				<view class="yzm">获取验证码</view>
			</view>
		</view>
	
		<view class="submitbtn" style="margin-top:140rpx ;">
			<button type="button"  @click="Router.switchTab({route:{path:'/pages/index/index'}})">确定</button>
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
			document.title = "注册"
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
	.pub_editInfor .item .name{width: 130rpx;}
	.pub_editInfor .item{border-bottom: 0;padding: 26rpx 4%;}
	.pub_editInfor .item .rr{border-bottom: 2rpx solid #eee;padding: 10rpx 0;}
</style>
