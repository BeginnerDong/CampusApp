<template>
	<view>
		
		<view class="flexRowBetween indexTit borderB1 W-Fixed">
			<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})"><image src="../../static/images/the-message-img.png" mode=""></image></view>
			<view class="fs16 color6">发现</view>
			<view @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})"><image class="seachBtn" src="../../static/images/home-icon.png" mode=""></image></view>
		</view>
		
		<view class="orderNav flexRowBetween whiteBj color6  W-Fixed borderB1" style="top: 98rpx;">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">社区</view>
			<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">已加入</view>
			<view class="tt flexCenter" :class="curr==3?'on':''" @click="changeCurr('3')">更多</view>
		</view>
		
		<view class="pdtb25"></view>
		<view class="pdtb20"></view>
		<view class="f5H5"></view>
		
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
				<view class="" @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
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
				<view  @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
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
		
		<view class="myRowBetween pdlr4" v-show="curr==2">
			<view class="item flexRowBetween" v-for="(item,index) in joinData" :KEY="index" @click="Router.navigateTo({route:{path:'/pages/communityHome/communityHome'}})">
				<view class="ll flex">
					<view class="photo"><image src="../../static/images/xiaoxi-img.png" mode=""></image></view>
					<view class="ll-tit">
						<view class="fs14">世界摄影大师</view>
						<view class="fs12 color9 mgt5">考研社区</view>
					</view>
				</view>
				<view class="rr">
					<view class="joinOK flexCenter color9 fs12">
						<image class="okIcon" src="../../static/images/xiaoxi-icon.png" mode=""></image>
						<text>已加入</text>
					</view>
				</view>
			</view>
		</view>
		<view class="myRowBetween pdlr4" v-show="curr==3">
			<view class="item flexRowBetween" v-for="(item,index) in joinData" :KEY="index">
				<view class="ll flex" @click="Router.navigateTo({route:{path:'/pages/communityHome/communityHome'}})">
					<view class="photo"><image src="../../static/images/xiaoxi-img.png" mode=""></image></view>
					<view class="ll-tit">
						<view class="fs14">世界摄影大师</view>
						<view class="fs12 color9 mgt5">考研社区</view>
					</view>
				</view>
				<view class="rr">
					<view class="joinOK color9 fs12"  @click="alertBoxShow">
						<text>加入</text>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 加入提示弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="alertBox center pdt20 radius10 whiteBj" v-show="is_alertBox">
			<view>提示：</view>
			<view class="pdt10 pdb20 color6 fs13">确定加入该社区？</view>
			<view class="flexRowBetween selt seltBtn fs13">
				<view class="half" @click="alertBoxShow">取消</view>
				<view class="half">确定</view>
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
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/find/find'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">发现</view>
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
				is_shareBtnShow:false,
				is_like:false,
				joinData:[{},{},{},{},{}],
				is_alertBox:false,
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
			alertBoxShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_alertBox = !self.is_alertBox
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
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/alertBox.css";
	
	page{padding-bottom: 140rpx;}
	
	.orderNav .tt.on:after{height: 0;}
	
	.swiper-box {height: 300rpx;box-sizing: border-box;overflow: hidden;border-radius: 10rpx;}
	.swiper-box swiper-item {width: 100%;}
	.swiper-box image{width: 100%;height: 100%;}
	
	.joinOK{padding: 0 10rpx;width: 140rpx;text-align: center;line-height: 50rpx;border-radius: 30rpx;border: 1px solid #eee;box-sizing: border-box;}
	.joinOK .okIcon{width: 21rpx;height: 15rpx;display: block;margin-right: 10rpx;}
</style>
