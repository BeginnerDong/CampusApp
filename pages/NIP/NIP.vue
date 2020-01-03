<template>
	<view>
		
		<view class="flexRowBetween indexTit borderB1  W-Fixed">
			<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})"><image src="../../static/images/the-message-img.png" mode=""></image></view>
			<view class="fs16 color6">NIP</view>
			<view @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})"><image class="seachBtn" src="../../static/images/home-icon.png" mode=""></image></view>
		</view>
		<view class="pdtb25"></view>
		
		<view class="pdlr4 whiteBj">
			<view class="banner-box pdt15">
				<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#222">
					<block v-for="(item,index) in labelData" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item" class="slide-image" />
						</swiper-item>
					</block>
				</swiper>
			</view>
		
			<view class="indHome flex fs13 pdt15">
				<view class="item"  @click="Router.navigateTo({route:{path:'/pages/foodLists/foodLists'}})">
					<image src="../../static/images/nip-icon.png"></image>
					<view class="tit">美食</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/foodLists/foodLists'}})">
					<image src="../../static/images/nip-icon1.png"></image>
					<view class="tit">休闲</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/foodLists/foodLists'}})">
					<image src="../../static/images/nip-icon2.png"></image>
					<view class="tit">娱乐</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/integralShop/integralShop'}})">
					<image src="../../static/images/nip-icon3.png"></image>
					<view class="tit">积分商城</view>
				</view>
			</view>
		</view>
		
		<view class="foodLis mglr4">
			<view class="item radius10 whiteBj" v-for="(item,index) in foodData" :key="index"  @click="Router.navigateTo({route:{path:'/pages/foodDetail/foodDetail'}})">
				<view class="fs15 ftw">绿茉莉 2-4人下午茶套餐</view>
				<view class="fs12 color6 pdtb5">陕西省西安市长安区韦曲街道</view>
				<view class="price fs16 ftw">128</view>
				<view class="ThreeImg flex">
					<view class="imgs"><image src="../../static/images/nip-img.png" mode=""></image></view>
					<view class="imgs"><image src="../../static/images/nip-img.png" mode=""></image></view>
					<view class="imgs"><image src="../../static/images/nip-img.png" mode=""></image></view>
				</view>
			</view>
		
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.navigateTo({route:{path:'/pages/activity/activity'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">活动</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/find/find'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">发现</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/NIP/NIP'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar4-a.png" />
				</view>
				<view class="text this-text">NIP</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/tidings/tidings'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar5.png" />
				</view>
				<view class="text">消息</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
				curr:1,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				foodData:[{},{},{}]
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
	@import "../../assets/style/head.css";
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/food.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	.swiper-box {height: 300rpx;box-sizing: border-box;overflow: hidden;border-radius: 10rpx;}
	.swiper-box swiper-item {width: 100%;}
	.swiper-box image{width: 100%;height: 100%;}
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 25%;text-align: center;padding-bottom: 15px; font-size: 24rpx; color: #222;display: flex; flex-direction: column;}
	.indHome .item image {width:90rpx;height: 90rpx; border-radius: 50%;margin: 0 auto 20rpx auto; }
	
</style>
