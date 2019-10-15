<template>
	<view>
		<view class="index_topBj">
			<view class="banner-box">
				<view class="banner oh">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000"  indicator-active-color="#434343">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item" class="slide-image"/>
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<view class="scrollMsg">
			<image class="icon" src="../../static/images/home-icon1.png" mode=""></image>
			<uni-notice-bar show-icon="false" scrollable="true" single="true" color="#999" :text="labelData.description">
				<view class="avoidOverflow">内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
			</uni-notice-bar>
		</view>
		
		<view class="f5H5"></view>
		<view class="infor-title flexRowBetween pr">
			<view class="tt">资讯</view>
			<view class="xian"></view>
			
		</view>
		<view class="pdlr4">
			<view class="ind_proList">
				<view class="item flexRowBetween" v-for="(item,index) in indProDate" :key="index" @click="Router.navigateTo({route:{path:'/pages/simpleDetail/simpleDetail'}})">
					<view class="ll">
						<image src="../../static/images/home-img.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow title">标题标题标题标题标题标题标题标题标题标题</view>
						<view class="avoidOverflow2 color3 font13 text">详情详情详情详情详情详情详情详情详情详情详情详情详情详情</view>
						<view class="data">2019-09-27</view>
					</view>
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
				showView: false,
				score:'',
				wx_info:{},
				scrollTop: 100,
				index: 0,
				text:"1233",
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
				],
				indProDate:[{},{},{},{}]
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/index.css";

	page{padding-bottom: 60rpx;}
	.swiper-box {height: 360rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item {width: 100%;}
	.swiper-box image {width: 100%;height: 100%;}
	
	.scrollMsg{line-height: 80rpx;padding: 0 3% 0 80rpx;position: relative;height: 80rpx;background: #fff;font-size: 26rpx;color: #666; position: relative;z-index: 2;}
	.scrollMsg .icon{ width: 38rpx; height:38rpx;position: absolute;top: 21rpx;left: 3%;}

</style>
