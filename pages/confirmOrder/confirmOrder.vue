<template>
	<view class="" >

		<view style="background: #fff;">
			
			<view class="cfmSetAdrs flexRowBetween" style="align-items: normal; border-bottom: 2rpx solid #e7e7e7;">
				<view class="yy-title">提货门店</view>
				<view class="yy-adrs flexRowBetween">
					<view class="cont font13">
						<view class="name" >门店名称门店名称&nbsp;&nbsp;&nbsp;&nbsp;15689795323</view>
						<view class="" >陕西省西安市雁塔区高新大都荟</view>
					</view>
					<view class="arrow" @click="Router.navigateTo({route:{path:'/pages/shopAddress/shopAddress'}})">
						<image class="arrowR" src="../../static/images/home-icon3.png" alt=""/>
					</view>
				</view>
			</view>
			
			<view class="ind_proList">
				<view class="item flexRowBetween" v-for="(item,index) in proListDate" :key="index">
					<view class="ll">
						<image src="../../static/images/img1.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">墨西哥牛油果8枚单果200g左右墨西哥牛油果</view>
						<view class="money flexRowBetween">
							<view class="flexRowBetween">
								<view class="price">{{item.price}}</view>
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
				proListDate:[
					{
						count:1,
						price:68
					},{
						count:1,
						price:50
					}
				],
				totalPrice:118
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '确认订单'
		},

		methods: {
			change(num){
				const self = this;
				if(num!=self.num){
					self.num = num
				}
			},
			
			counter(index,type) {
				const self = this;
				
				if (type == '+') {
					self.proListDate[index].count++;
				} else {
					if (self.proListDate[index].count > 1) {
						self.proListDate[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.proListDate[index], 'id', 999);
				self.countTotalPrice();
			},
			
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
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
