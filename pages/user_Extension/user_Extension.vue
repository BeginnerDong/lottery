<template>
	<view >
		<!--  -->
		<view class="tooling_indNav f5bj">
			<view class="list">
				<view class="item" :class="num==1?'on':''" @click="change('1')">推广团队</view>
				<view class="item" :class="num==2?'on':''" @click="change('2')">推广海报</view>
			</view>
		</view>
		<view class="myRowBetween" v-show="num==1">
			<view class="item flexRowBetween" v-for="(item,index) in xiaofeiData" :key="index">
				<view class="left flex">
					<image class="photo" src="../../static/images/to_promote-img.png" mode=""></image>
					<view>快乐的猫</view>
				</view>
				<view class="right"><view class="time">2019-10-15</view></view>
			</view>
		</view>
		<view class="pr"  v-show="num==2">
			<image class="poster" style="width: 100%;" src="../../static/images/to_promote-img1.png" mode="widthFix"></image>
		</view>
			
		<!-- 无数据时显示 -->
		<view class="noDataBox">
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
	/* page{padding-bottom: 140rpx;} */
	.tooling_indNav .list .item{width: 50%;}
	
	
</style>
