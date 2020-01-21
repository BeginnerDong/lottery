<template>
	<view>
		<view>
			<image style="width: 100%; height: 346rpx; display: block;padding-bottom: 60rpx;" src="../../static/images/registered-img.png" mode=""></image>
		</view>
		<view class="pub_editInfor">
			<view class="item">
				<view class="name">手机号：</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入手机号" v-model="submitData.phone"/>
				</view>
			</view>
			<view class="item">
				<view class="name">验证码：</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入验证码" v-model="submitData.code"/>
				</view>
			</view>
			<view class="item">
				<view class="name"></view>
				<view class="yzm"  @click="sendCode" v-if="!hasSend">{{text}}</view>
				<view class="yzm"  v-else>{{text}}</view>
			</view>
		</view>
	
		<view class="submitbtn" style="margin-top:140rpx ;">
			<button type="button"  open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确定</button>
		</view>
	
	</view>
</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					phone:'',
					code:''
				},
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
			}
		},


		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.phone;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					
					self.$Utils.showToast('请输入正确的手机号', 'none');
					return;
				}
				var postData = {
					data:{
						phone:self.submitData.phone
					}
					
				};
				var callback = function(res){
					if(res.solely_code==100000){
						self.hasSend = true;
						var interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
						
							self.text=self.currentTime + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(interval)
								
								self.hasSend = false;
								self.text='重新发送';
								self.currentTime= 61;
								
							}
							
						}, 1000);
					}else{
						self.$Utils.showToast('发送失败', 'none');
					};
				};
				self.$apis.codeGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					const callback = (user, res) => {
						self.userInfoUpdate();	
					};
					self.$Utils.getAuthSetting(callback);
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.smsAuth = {
					phone:self.submitData.phone,						
					code:self.submitData.code
				};
				postData.data = {
					phone:self.submitData.phone
				};
				//postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('注册成功', 'none');
						setTimeout(function() {
							self.Router.switchTab({route:{path:'/pages/index/index'}})
						}, 800)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
		}
	}
</script>

<style>
	.pub_editInfor .item .name{width: 130rpx;}
	.pub_editInfor .item{border-bottom: 0;padding: 26rpx 4%;}
	.pub_editInfor .item .rr{border-bottom: 2rpx solid #eee;padding: 10rpx 0;}
</style>
