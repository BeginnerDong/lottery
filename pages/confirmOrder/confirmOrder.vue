<template>
	<view class="" >
		<view style="background: #fff;">
			<view class="cfmSetAdrs flexRowBetween" style="align-items: normal; border-bottom: 2rpx solid #e7e7e7;">
				<view class="yy-title">提货门店</view>
				<view class="yy-adrs flexRowBetween">
					<view class="cont font13" v-if="addressData.name">
						<view class="name" >{{addressData.name}}&nbsp;&nbsp;&nbsp;&nbsp;{{addressData.phone}}</view>
						<view class="" >{{addressData.address}}</view>
					</view>
					<view class="cont" v-else>
						请选择提货门店
					</view>
					<view class="arrow" @click="Router.navigateTo({route:{path:'/pages/shopAddress/shopAddress'}})">
						<image class="arrowR" src="../../static/images/home-icon3.png" alt=""/>
					</view>
				</view>
			</view>
			
			<view class="ind_proList">
				<view class="item flexRowBetween" v-for="(item,index) in mainData.product" :key="index">
					<view class="ll">
						<image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">{{item.product?item.product.title:''}}</view>
						<view class="money flexRowBetween">
							<view class="flexRowBetween">
								<view class="price">{{item.product?item.product.price:''}}</view>
								<view class="numBox" style="position: absolute; right: 0; bottom: 0;">
									<view @click="counter(index,'-')">-</view>
									<view class="num">{{item.count}}</view>
									<view @click="counter(index,'+')">+</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="confm_BFix">
			合计： <view class="price">{{totalPrice}}</view>
			<view class="btn" @click="Utils.stopMultiClick(submit)">立即购买</view>
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils: this.$Utils,
				addressData:{},
				mainData:[],
				totalPrice:0,
				userInfoData:{},
				specialProvince:[],
				totalScore:0
			}
		},

		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick',true);
			self.mainData = uni.getStorageSync('payPro');
			console.log('self.data.mainData', self.mainData);
			const callback = (res) =>{
				self.$Utils.loadAll(['getUserInfoData'], self);	
			};
			self.$Token.getProjectToken(callback,{refreshToken:true})
			
			self.countTotalPrice();
		},

		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
			}
		},

		methods: {
			
			
			formIdAdd(e) {
				const self = this;
				console.log(e)
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};		
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData=res.info.data[0];	
					} else {
						self.$Utils.showToast(res.msg,'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},	
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				/* if(parseFloat(self.userInfoData.score)<parseFloat(self.totalScore)){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('积分不足','none');
					return
				}
				self.addOrder() */
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请选择提货门店','none')
				}else{
					if(parseFloat(self.userInfoData.score)<parseFloat(self.totalScore)){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('积分不足','none');
						return
					}
					self.addOrder()
				}
			},
			
			addOrder() {
				const self = this;					
				const postData = self.$Utils.cloneForm(self.mainData)
				postData.tokenFuncName = 'getProjectToken';	
				postData.data = {
					shop_no:self.addressData.user_no
				};
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					} else {		
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			
			pay(order_id) {
				const self = this;	
				const postData = {};	
				postData.score = {
					price:self.totalPrice
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user_exchange/user_exchange'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user_exchange/user_exchange'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
	
			
			counter(index,type) {
				const self = this;			
				if (type == '+') {
					self.mainData.product[index].count++;
				} else {
					if (self.mainData.product[index].count > 1) {
						self.mainData.product[index].count--;
					}
				};			
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;		
				self.totalScore = 0;
				for (var i = 0; i < self.mainData.product.length; i++) {
					self.totalPrice += self.mainData.product[i].product.price * self.mainData.product[i].count;
					self.totalScore += self.mainData.product[i].product.score * self.mainData.product[i].count;
				};
			},
			
			
		}
	}
</script>

<style>
	@import "../../assets/style/index.css";
	@import "../../assets/style/confirmOrder.css";
	page{background: #f5f5f5;padding-bottom: 140rpx;}

	.ind_proList .item .ll{width: 245rpx; height: 245rpx;}
	.ind_proList .item .rr{height: 245rpx;}
	.ind_proList .item .title{font-size: 28rpx; line-height: 40rpx;}
	.ind_proList .item .money{bottom: 20rpx;}
	
	.confm_BFix{ width: 100%; padding: 0 4%;box-sizing: border-box; line-height: 100rpx; box-shadow: 0 -3rpx 4rpx rgba(102,102,102,0.1); display: flex; justify-content: flex-end; font-size: 28rpx; align-items: center; position: fixed; bottom: 0; left: 0;background: #fff;}
	.confm_BFix .price{ font-size: 28rpx; color: #FF3B3B;}
	.confm_BFix .price::before{content: "￥"; font-weight: normal;}
	.confm_BFix .btn{ width: 200rpx; height: 70rpx; text-align: center; line-height: 70rpx; color: #fff; background: #ff2338; border-radius: 50rpx; margin-left: 20rpx;font-size: 30rpx;}

</style>
