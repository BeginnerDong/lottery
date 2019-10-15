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
			<view class="pdlr4">
				<view class="orderList boxShaow radius10">
					<view class="flexRowBetween">
						<view class="font12 color3">交易时间：2018-10-14</view>
						<view class="state">等待取货</view>
					</view>
					<view class="flex cont">
						<view class="leftImg"><image src="../../static/images/mall-img1.png" mode=""></image></view>
						<view class="right pr">
							<view class="title avoidOverflow2 font13">标题标题标题标题标题标题标题标题</view>
							<view class="price">59.00</view>
						</view>
					</view>
					<view class="code">兑换码：56123365</view>
				</view>
				
				<view class="orderList boxShaow radius10">
					<view class="flexRowBetween">
						<view class="font12 color3">交易时间：2018-10-14</view>
						<view class="state">已取货</view>
					</view>
					<view class="flex cont">
						<view class="leftImg"><image src="../../static/images/mall-img1.png" mode=""></image></view>
						<view class="right pr">
							<view class="title avoidOverflow2 font13">标题标题标题标题标题标题标题标题</view>
							<view class="price">59.00</view>
						</view>
					</view>
				</view>
			
				<!-- 无数据时显示 -->
				<view class="noDataBox">
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
				]
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
			
			
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
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
	.orderList .right .price{ position: absolute; bottom: 0;left: 0;}
	.price::before{content: "￥"}
	.orderList .code{ font-size: 24rpx;}
	
</style>
