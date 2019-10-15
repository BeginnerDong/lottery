<template>
	<view>
		
		<view class="proTopImg flexRowBetween">
			<view class="item-lis" v-for="(item,index) in produtList" :key="index" @click="Router.navigateTo({route:{path:'/pages/jiFenShopDetail/jiFenShopDetail'}})">
				<image class="img" src="../../static/images/mall-img1.png" alt="" />
				<view class="tit avoidOverflow2">墨西哥牛油8枚装单果200g左右</view>
				<view class="price">285</view>
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
				wx_info:{},
				is_show:false,
				produtList:[
					{},{},{},{},{},{}
				]
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
	.proTopImg{padding: 20rpx 4%;flex-wrap: wrap; background: #F5F5F5;}
	.proTopImg .item-lis{ width: 330rpx; overflow: hidden;background: #fff;margin: 15rpx 0; position: relative;border-radius: 10rpx; padding-bottom: 20rpx;}
	.proTopImg .item-lis .img{width: 100%;height: 330rpx;display: block;margin: 0 auto;}
	.proTopImg .item-lis .tit{ font-size: 26rpx; padding: 20rpx 4% 20rpx 4%; line-height: 32rpx; height: 64rpx;}
	.proTopImg .item-lis .price{font-size: 28rpx;padding: 0 4%; color: #ff3b3b;}
	.proTopImg .item-lis .price:before{content: "积分："; font-size: 24rpx;}
</style>
