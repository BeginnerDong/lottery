<template>
	<view class="" >
		<!--  -->
		<view class="borderB1"></view>
		
			<view class="tooling_indNav f5bj">
				<view class="list">
					<view class="item" :class="num==1?'on':''" @click="change('1')">全部</view>
					<view class="item" :class="num==2?'on':''" @click="change('2')">待取货</view>
					<view class="item" :class="num==3?'on':''" @click="change('3')">已取货</view>
				</view>
			</view>
			<view class="pdlr4" v-if="mainData.length>0">
				<view class="orderList boxShaow radius10" v-for="item in mainData">
					<view class="flexRowBetween">
						<view class="font12 color3">交易时间：{{item.create_time}}</view>
						<view class="state" v-if="item.check_status==0">等待取货</view>
						<view class="state" v-if="item.check_status==1">已取货</view>
					</view>
					<view class="flexRowBetween" style="line-height: 60rpx;">
						<view class="font12 color3">取货门店：{{item.shop?item.shop.name:''}}</view>
					</view>
					<view class="flex cont" v-for="c_item in item.products">
						<view class="leftImg">
							<image :src="c_item.snap_product&&c_item.snap_product.mainImg&&c_item.snap_product.mainImg[0]?c_item.snap_product.mainImg[0].url:''" mode="">
								
							</image>
						</view>
						<view class="right pr">
							<view class="title avoidOverflow2 font13" style="height: 150rpx;">{{c_item.snap_product?c_item.snap_product.title:''}}</view>
							<view class="flexRowBetween">
								<view class="">x{{c_item?c_item.count:''}}</view>
								<view class="price">{{item.price}}</view>
							</view>
						</view>
					</view>
					<view class="code" v-if="item.check_status==0">兑换码：{{item.check_no}}</view>
				</view>
				
				
			
				<!-- 无数据时显示 -->
				<view class="noDataBox" v-if="mainData.length==0">
					<image src="../../static/images/nodata.png" mode="widthFix"></image>
				</view>
			</view>	
	</view>

</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				num:1,
				proListDate:[
					{
						count:1,
						price:68
					},{
						count:1,
						price:50
					}
				],
				paginate:{
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 10
				},
				mainData:[],
				searchItem:{
					thirdapp_id:2
				}
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
			change(num){
				const self = this;
				if(num!=self.num){
					self.num = num
					if(num==1){
						delete self.searchItem.check_status
					}else if(num==2){
						self.searchItem.check_status=0
					}else{
						self.searchItem.check_status=1
					}
					self.getMainData(true)
				}
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
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					shop:{
						tableName:'UserInfo',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{status:1},
						condition:'=',
						info:['name']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/NavTab.css";
	page{padding-bottom: 140rpx;}


	/* 产品列表 */
	.orderList {padding: 30rpx; margin-top: 30rpx;}
	.state{color:#ff2e34; font-size: 24rpx;}
	.orderList .cont{ margin: 20rpx 0; align-items:flex-start;}
	.orderList .leftImg{ width: 190rpx; height: 190rpx; margin-right: 20rpx;border-radius: 10rpx;overflow: hidden;}
	.orderList .leftImg image{width: 100%; height: 100%; display: block;}
	.orderList .right{width: 410rpx; height: 190rpx;}
	
	.price::before{content: "￥"}
	.orderList .code{ font-size: 24rpx;}
	
</style>
