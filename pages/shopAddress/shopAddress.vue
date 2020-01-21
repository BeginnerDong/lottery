<template>
	<view class="">
		<view class="shopAddres">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index" @click="seltCurr(index)">
				<view class="infor">
					<view class="flex">
						<image class="icon" src="../../static/images/stores-icon1.png" mode=""></image>
						<view class="title font14">{{item.name}}</view>
						<view class="phone color3 font13">{{item.phone}}</view>
					</view>
					<view class="adrs font13 color3">{{item.address}}</view>
				</view>
				<view class="arrow flexCenter">
					<image class="" :src="setCurr == index?'../../static/images/stores-icon2.png':'../../static/images/stores-icon3.png'"  mode=""></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				setCurr:-1,
				shopAddres:[
					{},{},{}
				],
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 10
				},
			}
		},

		onLoad(options) {
			const self = this;
			//self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
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
			
			seltCurr(index){
				const self = this;
				self.setCurr = index
				uni.setStorageSync('choosedAddressData', self.mainData[index]);
				console.log('choosedIndex', self.choosedIndex);
				uni.navigateBack({
					delta:1
				})
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					user_type:1
				};
				postData.getBefore = {
					user:{
						tableName:'User',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:['=',[1]],
							primary_scope:['=',[60]]
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
		}
	}
</script>

<style>
	
	page{background: #f5f5f5;padding-bottom: 140rpx;}
	.shopAddres .item{ padding: 30rpx 4%;background: #fff; margin-bottom: 10rpx;}
	.shopAddres .item .infor{ width: 88%;}
	.shopAddres .item .arrow{ width: 12%; justify-content: flex-end;}
	.shopAddres .item .icon{width: 30rpx; height: 30rpx; display: block; margin-right: 10rpx;}
	.shopAddres .item .arrow image{width: 40rpx; height: 40rpx; display: block;}
	.shopAddres .adrs{ margin-top: 16rpx; }
	.shopAddres .title{ margin: 0 20rpx 0 4rpx; }
	
</style>
