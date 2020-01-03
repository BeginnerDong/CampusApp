<template>
	<view>
		
		<view class="flexRowBetween indexTit borderB1 W-Fixed">
			<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})"><image src="../../static/images/the-message-img.png" mode=""></image></view>
			<view class="titNav fs16 color6 flexRowBetween">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">推荐</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">关注</view>
			</view>
			<view @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})"><image class="seachBtn" src="../../static/images/home-icon.png" mode=""></image></view>
		</view>
		
		<view class="pdtb25"></view>
		
		<view class="fabubtn" @click="Router.navigateTo({route:{path:'/pages/persDynamic/persDynamic'}})"><image class="icon" src="../../static/images/home-icon1.png" mode=""></image></view>
		
		<view class="banner-box pdt15 mglr4">
			<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#222">
				<block v-for="(item,index) in labelData" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		
		<view class="comment mglr4" v-show="curr==1">
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
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
				<view class="" @click="Router.navigateTo({route:{path:'/pages/postDetails/postDetails'}})">
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
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
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex">
						<image src="../../static/images/home-icon3.png" mode=""></image>
						<view>265</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon5.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="dian"></view>
						<view class="dian"></view>
						<view class="dian"></view>
					</view>
				</view>
				<view class="" @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
					<view class="imgbox">
						<view class="img lisTwo">
							<image src="../../static/images/the-store-img2.png" mode=""></image>
						</view>
						<view class="img lisTwo">
							<image src="../../static/images/the-store-img2.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex">
						<image src="../../static/images/home-icon4.png" mode=""></image>
						<view>18</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon3.png" mode=""></image>
						<view>265</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon6.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="dian"></view>
						<view class="dian"></view>
						<view class="dian"></view>
					</view>
				</view>
				<view class="" @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
					<view class="imgbox">
						<view class="img lisOne">
							<image src="../../static/images/home-img3.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex">
						<image src="../../static/images/home-icon4.png" mode=""></image>
						<view>18</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon3.png" mode=""></image>
						<view>265</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon6.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="dian"></view>
						<view class="dian"></view>
						<view class="dian"></view>
					</view>
				</view>
				<view  @click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail'}})">
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科回复第三规范化供货方的是胡椒粉人工湖</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex">
						<image src="../../static/images/activity-icon2.png" mode=""></image>
						<view>收藏</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon6.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="comment mglr4" v-show="curr==2">
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="dian"></view>
						<view class="dian"></view>
						<view class="dian"></view>
					</view>
				</view>
				<view class="" @click="Router.navigateTo({route:{path:'/pages/postDetails/postDetails'}})">
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
					<view class="imgbox">
						<view class="img lisThree">
							<image src="../../static/images/home-img2.png" mode=""></image>
						</view>
						<view class="img lisThree">
							<image src="../../static/images/home-img2.png" mode=""></image>
						</view>
						<view class="img lisThree">
							<image src="../../static/images/home-img2.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex">
						<image src="../../static/images/home-icon3.png" mode=""></image>
						<view>265</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon5.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="dian"></view>
						<view class="dian"></view>
						<view class="dian"></view>
					</view>
				</view>
				<view class="" @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
					<view class="imgbox">
						<view class="img lisTwo">
							<image src="../../static/images/home-img2.png" mode=""></image>
						</view>
						<view class="img lisTwo">
							<image src="../../static/images/home-img2.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex">
						<image src="../../static/images/home-icon4.png" mode=""></image>
						<view>18</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon3.png" mode=""></image>
						<view>265</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon6.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="dian"></view>
						<view class="dian"></view>
						<view class="dian"></view>
					</view>
				</view>
				<view class="" @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
					<view class="imgbox">
						<view class="img lisOne">
							<image src="../../static/images/home-img3.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex">
						<image src="../../static/images/home-icon4.png" mode=""></image>
						<view>18</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon3.png" mode=""></image>
						<view>265</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon6.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="dian"></view>
						<view class="dian"></view>
						<view class="dian"></view>
					</view>
				</view>
				<view  @click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail'}})">
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科回复第三规范化供货方的是胡椒粉人工湖</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex">
						<image src="../../static/images/activity-icon2.png" mode=""></image>
						<view>收藏</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon6.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.navigateTo({route:{path:'/pages/activity/activity'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">活动</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/find/find'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">发现</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/NIP/NIP'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar4.png" />
				</view>
				<view class="text">NIP</view>
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
				is_shareBtnShow:false,
				is_like:false
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
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
	@import "../../assets/style/head.css";
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	
	page{padding-bottom: 140rpx;}
	.swiper-box {height: 300rpx;box-sizing: border-box;overflow: hidden;border-radius: 10rpx;}
	.swiper-box swiper-item {width: 100%;}
	.swiper-box image{width: 100%;height: 100%;}
</style>
