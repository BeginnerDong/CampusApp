<template>
	<view>
		
		<view class="flexRowBetween indexTit borderB1  W-Fixed" style="padding-top: 44px;">
			<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})">
				<image :src="userInfoData.mainImg&&userInfoData.mainImg.length>0?userInfoData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
			</view>
			<view class="fs16 color6">NIP</view>
			<view @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})"><image class="seachBtn" src="../../static/images/home-icon.png" mode=""></image></view>
		</view>
		<view style="height: 44px;background-color: #fff;"></view>
		<view class="pdtb25"></view>
		
		<view class="pdlr4 whiteBj">
			<view class="banner-box pdt15">
				<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#222">
					<block v-for="(item,index) in sliderData" :key="index">
						<swiper-item class="swiper-item" :data-url="item.url" @click="Router.navigateTo({route:{path:$event.currentTarget.dataset.url}})">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image" />
						</swiper-item>
					</block>
				</swiper>
			</view>
		
			<view class="indHome flex fs13 pdt15">
				<view class="item"  @click="Router.navigateTo({route:{path:'/pages/foodLists/foodLists?name=美食'}})">
					<image src="../../static/images/nip-icon.png"></image>
					<view class="tit">美食</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/foodLists/foodLists?name=休闲'}})">
					<image src="../../static/images/nip-icon1.png"></image>
					<view class="tit">休闲</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/foodLists/foodLists?name=娱乐'}})">
					<image src="../../static/images/nip-icon2.png"></image>
					<view class="tit">娱乐</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/integralShop/integralShop'}})">
					<image src="../../static/images/nip-icon3.png"></image>
					<view class="tit">积分商城</view>
				</view>
			</view>
		</view>
		
		<view class="foodLis mglr4" v-if="mainData.length>0">
			<view class="item radius10 whiteBj" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/foodDetail/foodDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="fs15 ftw">{{item.title}}</view>
				<view class="fs12 color6 pdtb5">{{item.shop?item.shop.address:''}}</view>
				<view class="price fs16 ftw">{{item.price}}</view>
				<view class="ThreeImg flex">
					<view class="imgs" v-for="c_item in item.mainImg"><image :src="c_item.url" mode=""></image></view>
					<!-- <view class="imgs"><image src="../../static/images/nip-img.png" mode=""></image></view>
					<view class="imgs"><image src="../../static/images/nip-img.png" mode=""></image></view> -->
				</view>
			</view>
		</view>
		<view v-else>
			<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
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
				foodData:[{},{},{}],
				sliderData:[],
				mainData:[],
				searchItem:{
					type:1,
					thirdapp_id:2,
					on_shelf:1
				},
				userInfoData:{}
			}
		},
		
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData','getSliderData','getMainData'], self);
			// self.$Utils.loadAll(['getMainData'], self);
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
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
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
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['in', ['NIP轮播']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData.push.apply(self.sliderData, res.info.data)
					}
					console.log('self.sliderData', self.sliderData)
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					shop:{
						token:uni.getStorageSync('user_token'),
						tableName:'UserInfo',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['address']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
			
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
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
