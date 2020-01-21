<template>
	<view>
		<form @submit="formIdAdd" report-submit="true">
		<view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		
		<view class="detail_one">
			<view class="tit font14">{{mainData.title}}</view>
			<view class="flex">
				
			</view>
			<view class="flexRowBetween color2 font13 number">
				<view class="price font14">{{mainData.price}}</view>
				<view class="red">销量:{{mainData.sale_count}}</view>
			</view>
			
		</view>
		
		<view class="f5H5"></view>
		<view class="infor-title">
			<view class="tt">详情介绍</view>
		</view>
		<view class="xqInfor">
			<view class="content ql-editor"  v-html="mainData.content">
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar">
			<view class="left flexRowBetween">
				<view class="ite">
					<image src="../../static/images/details-icon.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<button class="payBtn flexRowBetween"  form-type="submit" style="font-size: 14px;"   @click="goBuy()">
				<view class="item hei" style="width: 100%;">
					立即兑换
				</view>
			</button>
		</view>
		</form>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		
			formIdAdd(e) {
				const self = this;
				console.log(e)
			
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:self.id
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			goBuy() {
				const self = this;
				if (JSON.stringify(self.mainData) == '{}') {
					api.showToast('商品错误', 'none', 1000);
					return;
				};
				var orderList = {
					product: [{
						id: self.mainData.id,
						count: 1,
						product: self.mainData
					}],
					type:self.mainData.type,
				};
				uni.setStorageSync('payPro', orderList);
				self.$Router.navigateTo({
					route: {
						path: '/pages/confirmOrder/confirmOrder'
					}
				})
			},

		},
	};
</script>

<style>
	@import "../../assets/style/prodctDetail.css";
	@import "../../assets/style/quill.css";
	page{padding-bottom: 140rpx;}
	.price::before{content: "积分："; font-size: 24rpx;}
	

</style>
