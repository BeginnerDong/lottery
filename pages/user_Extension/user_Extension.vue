<template>
	<view >
		<!--  -->
		<view class="tooling_indNav f5bj" style="padding: 15rpx 4%;">
			<view class="list">
				<view class="item" :class="num==1?'on':''" @click="change('1')">推广团队</view>
				<view class="item" :class="num==2?'on':''" @click="change('2')">推广海报</view>
			</view>
		</view>
		<view class="myRowBetween" v-if="num==1">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="left flex">
					<image class="photo" :src="item.user&&item.user[0]?item.user[0].headImgUrl:''" mode=""></image>
					<view>{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
				</view>
				<view class="right"><view class="time">{{item.create_time}}</view></view>
			</view>
		</view>
		<view class="pr"  v-if="num==2">
			<view style="position: relative;">
				<image  :src="QrData.url" mode="widthFix" style="position: absolute;bottom: 40px;width:50%;left:25%;height:175px"></image>
				<image class="poster" style="width: 100%;" src="../../static/images/to_promote-img1.png" mode="widthFix"></image>
			</view>
			
			
		</view>
			
		<!-- 无数据时显示 -->
		<view class="noDataBox" v-if="num==1&&mainData.length==0">
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
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
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 10
				},
				QrData:{}
			}
		},

		onLoad(options) {
			const self = this;
			//self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getQrData'], self);
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
				postData.searchItem = {
					thirdapp_id: 2,
					parent_no:uni.getStorageSync('user_info').user_no,
				};
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'child_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.distriGet(postData, callback);
			},
			
			getQrData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: uni.getStorageSync('user_info').user_no,
					path: 'pages/index/index',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.QrData = res.info;
					}
					self.$Utils.finishFunc('getQrData');
				};
				self.$apis.getQrCode(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/NavTab.css";
	/* page{padding-bottom: 140rpx;} */
	.tooling_indNav .list .item{width: 50%;}
	
	
</style>
