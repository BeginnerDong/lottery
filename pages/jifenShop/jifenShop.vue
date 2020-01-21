<template>
	<view>
		
		<view class="proTopImg flexRowBetween">
			<view class="item-lis" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/jiFenShopDetail/jiFenShopDetail?id='+$event.currentTarget.dataset.id}})">
				<image class="img" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
				<view class="tit avoidOverflow2">{{item.title}}</view>
				<view class="price">{{item.price}}</view>
			</view>
		</view>
	</view>
</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[]
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},

		methods: {
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5	
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	}
</script>

<style>
	page{
		background: #f5f5f5;
	}
	.proTopImg{padding: 20rpx 4%;flex-wrap: wrap; background: #F5F5F5;}
	.proTopImg .item-lis{ width: 330rpx; overflow: hidden;background: #fff;margin: 15rpx 0; position: relative;border-radius: 10rpx; padding-bottom: 20rpx;}
	.proTopImg .item-lis .img{width: 100%;height: 330rpx;display: block;margin: 0 auto;}
	.proTopImg .item-lis .tit{ font-size: 26rpx; padding: 20rpx 4% 20rpx 4%; line-height: 32rpx; height: 64rpx;}
	.proTopImg .item-lis .price{font-size: 28rpx;padding: 0 4%; color: #ff3b3b;}
	.proTopImg .item-lis .price:before{content: "积分："; font-size: 24rpx;}
</style>
