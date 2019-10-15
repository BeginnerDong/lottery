<template>
	<view >
		<!--  -->
		<view class="borderB1"></view>
		<view class="pr" style="background: #ff2e34; width: 100%; height: 256rpx; margin-bottom: 80rpx;">
			<view class="boxShaow radius10 integralData flexRowBetween">
				<view class="item center">
					<view class="number">300</view>
					<view class="font12 color2">余额积分</view>
				</view>
				<view class="item center">
					<view class="number" >
						<text class="pr"  @click="showRemarks">100<image style="width: 22rpx; height: 22rpx; display: block; position: absolute; top: -10rpx;right: -40rpx;padding: 10rpx;" src="../../static/images/icon10.png" mode=""></image></text>
					</view>
					<view class="font12 color2">在途积分</view>
				</view>
			</view>
		</view>
		
		<view class="f5bj">
			<view class="orderNav">
				<view class="tt" :class="num==1?'on':''" @click="change('1')">消费积分流水</view>
				<view class="tt" :class="num==2?'on':''" @click="change('2')">推广积分流水</view>
			</view>
		</view>
		
		<view class="myRowBetween" v-show="num==1">
			<view class="item flexRowBetween" v-for="(item,index) in xiaofeiData" :key="index">
				<view class="left">
					<view>兑换商品</view>
					<view class="time">2019-10-15</view>
				</view>
				<view class="right">-532</view>
			</view>
		</view>
		<view class="myRowBetween"  v-show="num==2">
			<view class="item flexRowBetween" v-for="(item,index) in tuiguangData" :key="index">
				<view class="left">
					<view>张丹</view>
					<view class="time">2019-10-15</view>
				</view>
				<view class="right">+668</view>
			</view>
		</view>
			
		<!-- 无数据时显示 -->
		<view class="noDataBox">
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
		</view>
		
		<view class="xieyiAlert" v-show="is_show">
			<view class="infor">
				<view class="colseBtn" @click="showRemarks" style="top: auto; left: 50%;bottom: 50rpx; line-height: 40rpx;">x</view>
				<view class="cont">
					<view class="tit center">备注说明</view>
					<view>1、内容内容内容内容内容内容内容内容内容内容 </view>
					<view>2、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
					<view>3、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容 </view>
					<view>4、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容 </view>
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
				num:1,
				xiaofeiData:[{},{},{}],
				tuiguangData:[{},{},{}],
				is_show:false
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
			showRemarks(){
				const self = this;
				self.is_show = !self.is_show
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
	.integralData{background: #fff; padding: 40rpx 0;position: absolute;top: 130rpx; width:92% ;left: 50%;transform: translateX(-50%);}
	.integralData .item{width: 50%;padding: 0 10rpx;box-sizing: border-box;}
	.integralData .item:first-child{border-right: 1px solid #eee;}
	.integralData .item .number{margin-bottom: 10rpx; font-size: 38rpx;}
	
	.orderNav{padding: 10rpx 0;}
	.orderNav .on::after{width: 100rpx;}
	
</style>
