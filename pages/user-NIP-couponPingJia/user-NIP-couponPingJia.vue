<template>
	<view>
		<view class="mglr4 radius10 whiteBj mgt15 canTing">
			<view class="flex">
				<view class="pic">
					<image src="../../static/images/coupons-img.png" mode=""></image>
				</view>
				<view class="infor mgl10  fs12 color6" style="width: 70%;">
					<view class="fs14 color2 ftw">绿茉莉</view>
					<view class="flex" style="flex-wrap: wrap;">
						<view class="lable">甜点</view>
					</view>
					<view class="adrs mgt15">南稍门 5.1km</view>
				</view>
			</view>
		</view>
		
		<view class="mglr4 whiteBj radius8 mgt15 pdlr4">
			<view class="pdtb15 flex borderB1">
				<view class="mgr10 fs13">总体</view>
				<view class="starBox mgr5">
					<image src="../../static/images/the-store-icon4.png" mode=""></image>
					<image src="../../static/images/the-store-icon5.png" mode=""></image>
					<image src="../../static/images/the-store-icon5.png" mode=""></image>
					<image src="../../static/images/the-store-icon5.png" mode=""></image>
					<image src="../../static/images/the-store-icon5.png" mode=""></image>
				</view>
			</view>
			<view class="borderB1 mgb15">
				<textarea class="fs12 " style="height: 240rpx;" value="" placeholder="说几句,让更多小伙伴了解我" />
			</view>
			<view class="oh">
				<block v-for="(image,index) in imageList" :key="index">
					<view class="picRow">
						<image :src="image" :data-src="image" @tap="previewImage"></image>
					</view>
				</block>
				<view class="picRow" @tap="chooseImage"><image src="../../static/images/release-icon.png" mode=""></image></view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<view class="btn">发表</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
				imageList: []
			}
		},
		onUnload() {
			this.imageList = []
		},
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			chooseImage: async function() {
				uni.chooseImage({
					success: (res) => {
						this.imageList = this.imageList.concat(res.tempFilePaths);
					}
				})
			},
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/star.css";
	@import "../../assets/style/foodDetail.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.canTing{padding: 30rpx 4%;}
	.starBox image{width: 32rpx;height: 32rpx;margin-right: 8rpx;}
	textarea{padding:20rpx 0;}
	
	.picRow{margin: 0 20rpx 20rpx 0;float: left;width: 140rpx;height: 140rpx;background: #F5F5F5;}
	.picRow:nth-of-type(4n){margin-right: 0;}
	.picRow image{width: 100%;height: 100%; display: block;}
</style>
