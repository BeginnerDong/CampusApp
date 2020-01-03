<template>
	<view>
		
		<view class="comment mglr4">
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<!-- <view class="dian"></view>
						<view class="dian"></view>
						<view class="dian"></view> -->
					</view>
				</view>
				<view class="font14 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
				<view class="imgbox">
					<view class="img lisThree">
						<image src="../../static/images/the-store-img2.png" mode=""></image>
					</view>
					<view class="img lisThree">
						<image src="../../static/images/the-store-img2.png" mode=""></image>
					</view>
					<view class="img lisThree">
						<image src="../../static/images/the-store-img2.png" mode=""></image>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="pdlr4">
			<view class="pdt15 ftw">评论</view>
			<view class="pinglunLis">
				<view class="item" v-for="(item,index) in pinglunData" :key="index">
					<view class="flex">
						<view class="pler-photo"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name mgl10">
							<view class="fs12 color6">小白</view>
							<view class="fs10 color9">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="pdtb10 fs12">监管科洛杉矶港就赶快来角色管理发送警告上了感觉监管科粉丝滤镜的规范是牢固树立感觉</view>
					<view class="flexRowBetween">
						<view class="flex fs11 numHF"  @click="Router.navigateTo({route:{path:'/pages/post-plDetail/post-plDetail'}})">80条回复&gt;</view>
						<view class="flexEnd fs12">
							<image class="plIcon mgr10" src="../../static/images/details-icon.png" mode=""></image>
							<text>回复</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="fx-PlZan pdlr4 flexRowBetween fs12 color6">
			<view class="tt flexCenter">
				<image class="icon" src="../../static/images/home-icon3.png" mode=""></image>
				<text>评论</text>
			</view>
			<view class="tt flexCenter">
				<image class="icon" src="../../static/images/home-icon5.png" mode=""></image>
				<text>赞</text>
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
				wx_info:{},
				is_show:false,
				pinglunData:[{},{},{}]
			}
		},
		onLoad() {
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
			}
		}
	};
</script>

<style>
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	page{padding-bottom: 100rpx;}
	.comment .child{border-bottom: 0;}
	
</style>
