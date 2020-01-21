<template>
	<view>
		<view v-if="showAll">
			<view class="index_topBj">
				<view class="banner-box">
					<view class="banner oh">
						<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000"
						 indicator-active-color="#434343">
							<block v-for="(item,index) in swiperData.mainImg" :key="index">
								<swiper-item class="swiper-item">
									<image :src="item.url" class="slide-image" />
								</swiper-item>
							</block>
						</swiper>
					</view>
				</view>
			</view>
			<view class="scrollMsg">
				<image class="icon" src="../../static/images/home-icon1.png" mode=""></image>
				<!-- <uni-notice-bar show-icon="false" scrollable="true" single="true" color="#999" 
				backgroundColor="#fff" :text="swiperData.description">
				</uni-notice-bar> -->
				<cmd-notice-bar backgroundColor="#fff" scrollable :text="swiperData.description"></cmd-notice-bar> 
			</view>
			
			<view class="f5H5"></view>
			<view class="infor-title flexRowBetween pr">
				<view class="tt">资讯</view>
				<view class="xian"></view>
			
			</view>
			<view class="pdlr4">
				<view class="ind_proList">
					<view class="item flexRowBetween" :data-id="item.id" v-for="(item,index) in mainData" :key="index" 
					@click="Router.navigateTo({route:{path:'/pages/simpleDetail/simpleDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="ll">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow title">{{item.title}}</view>
							<view class="avoidOverflow2 color3 font13 text">{{item.description}}</view>
							<view class="data">{{item.create_time}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	/* import uniNoticeBar from "@/components/uni-notice-bar/uni-notice-bar" */
	import cmdNoticeBar from "@/components/cmd-notice-bar/cmd-notice-bar.vue"
	
	export default {
		
		components: {cmdNoticeBar},
		
		data() {
			return {
				Router: this.$Router,
				mainData: [],
				swiperData: {},
				showAll:false
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			if(options.scene){
				var scene = decodeURIComponent(options.scene);
				const callback = (res) => {
					self.$Utils.loadAll(['getUserInfoData', 'getSliderData', 'getMainData'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true,parent_no:scene
				})
			}else{
				const callback = (res) => {
					self.$Utils.loadAll(['getUserInfoData', 'getSliderData', 'getMainData'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true
				})
			}
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
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['资讯']],
						},
						middleKey: 'menu_id',
						key: 'id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},

			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						if(self.userInfoData.phone==''){
							self.Router.redirectTo({route:{path:'/pages/register/register'}})
						}else{
							self.showAll = true
						}
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '首页轮播'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.swiperData = res.info.data[0]
					}
					console.log('self.swiperData', self.swiperData)
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/index.css";

	page {
		padding-bottom: 60rpx;
	}

	.swiper-box {
		height: 360rpx;
		box-sizing: border-box;
		overflow: hidden;
	}

	.swiper-box swiper-item {
		width: 100%;
	}

	.swiper-box image {
		width: 100%;
		height: 100%;
	}

	.scrollMsg {
		line-height: 80rpx;
		padding: 0 3% 0 80rpx;
		position: relative;
		height: 80rpx;
		background: #fff;
		font-size: 26rpx;
		color: #666;
		position: relative;
		z-index: 2;
	}

	.scrollMsg .icon {
		width: 38rpx;
		height: 38rpx;
		position: absolute;
		top: 21rpx;
		left: 3%;
	}
</style>
