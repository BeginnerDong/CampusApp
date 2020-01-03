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
					<view class="pr">
						<view class="flexEnd" @click="shareBtnShow">
							<view class="dian"></view>
							<view class="dian"></view>
							<view class="dian"></view>
						</view>
						<view class="fx-shareBtn fs11 white" v-show="is_shareBtnShow">
							<view class="flexCenter pdb10" @click="like">
								<image class="icon" :src="is_like==true?'../../static/images/home-icon9.png':'../../static/images/home-icon7.png'" mode=""></image>
								<text>收藏</text>
							</view>
							<view class="flexCenter" @click="Router.navigateTo({route:{path:'/pages/report/report'}})">
								<image class="icon" src="../../static/images/home-icon8.png" mode=""></image>
								<text>举报</text>
							</view>
						</view>
					</view>
					
				</view>
				<view class="font14 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
				<view class="imgbox">
					<view class="img lisOne">
						<image src="../../static/images/home-img3.png" mode=""></image>
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
				<image class="icon" src="../../static/images/home-icon4.png" mode=""></image>
				<text>分享</text>
			</view>
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
				pinglunData:[{},{},{}],
				is_shareBtnShow:false,
				is_like:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			shareBtnShow(){
				const self = this;
				self.is_shareBtnShow = !self.is_shareBtnShow
			},
			like(){
				const self = this;
				self.is_like = !self.is_like;
				self.is_shareBtnShow = !self.is_shareBtnShow
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
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	page{padding-bottom: 100rpx;}
	.comment .child{border-bottom: 0;}
	.fx-shareBtn{width: 200rpx;padding: 30rpx 0rpx;border-radius: 10rpx;background: rgba(0,0,0,0.5);position: absolute;top: 30rpx;right: 10rpx;z-index: 2;}
	.fx-shareBtn .icon{width: 28rpx;height: 28rpx;margin-right: 10rpx; }
</style>
