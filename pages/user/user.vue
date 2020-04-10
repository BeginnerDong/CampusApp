<template>
	<view>
		<view class="">
			<view class="userHead white" style="">
				<view class="flexRowBetween pdlr4 pdt15 pdb10 white" style="z-index: 2;padding-top: 44px;border-bottom: 1px solid #4b4b4b;position: fixed;top: 0;width: 92%;background-color: #323232;">
					<view @click="prev()"><image class="arrowR" style="margin-left: 0;" src="../../static/images/arrowL.png" mode=""></image></view>
					<view class="fs15">我的</view>
					<view @click="Router.navigateTo({route:{path:'/pages/userSetting/userSetting'}})"><image style="width: 32rpx;height: 34rpx;display: block;" src="../../static/images/about-icon.png" mode=""></image></view>
				</view>
				<view style="height: 83px;"></view>
				<view class="infor pdt25 pdlr4">
					<view class="left flex">
						<view v-if="mainData.check_status==2" @click="Router.navigateTo({route:{path:'/pages/personHome/personHome'}})">
							<image class="photo" :src="mainData&&mainData.mainImg&&mainData.mainImg.length>0?mainData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view v-if="mainData.check_status!=2" @click="realnameshow">
							<image class="photo" :src="mainData&&mainData.mainImg&&mainData.mainImg.length>0?mainData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view style="width: 70%;">
							<view class="fs16 pdb10">{{mainData.name}}</view>
							<view class="flex">
								<view class="userLable color6 fs11">{{mainData.role}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="myRowBetween fs13 mglr4 mgt25 boxShaow radius10 whiteBj">
				<view class="item flexRowBetween" v-if="mainData.check_status==2"
				@click="showToast()" >
					<view class="ll flex">实名认证</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" v-if="mainData.check_status!=2"
				@click="Router.navigateTo({route:{path:'/pages/user-realname/user-realname'}})" >
					<view class="ll flex">实名认证</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-NIP/user-NIP'}})" >
					<view class="ll flex">NIP值</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myFabu/user-myFabu'}})" >
					<view class="ll flex">我的发布</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myFollow/user-myFollow'}})" >
					<view class="ll flex">我的关注</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-mySheQu/user-mySheQu'}})" >
					<view class="ll flex">我的社区</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-addSheQu/user-addSheQu'}})" >
					<view class="ll flex">创建社区</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-joinActivity/user-joinActivity'}})" >
					<view class="ll flex">参与活动</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-order/user-order'}})" >
					<view class="ll flex">订单信息</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-collect/user-collect'}})" >
					<view class="ll flex">收藏夹</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-joinUs/user-joinUs'}})" >
					<view class="ll flex">加入我们</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-feedback/user-feedback'}})" >
					<view class="ll flex">意见反馈</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		
		<view class="realnameshow whiteBj radius10 pdb25" v-show="is_realnameshow">
			<view class="closebtn" style="z-index: 2;" @click="realnameshow">×</view>
			<view>
				<image class="bjImg w" src="../../static/images/about-icon2.png" mode="widthFix"></image>
			</view>
			<view class="pdt25 pdb15 center fs18 ftw">实名认证</view>
			<view class="fs12 pdl20 pdr20">您还未认证身份信息，为方便您使用与享受更多的特权服务，请前去认证身份，完善资料。</view>
			<view class="submitbtn mgt25 mgb25" @click="Router.navigateTo({route:{path:'/pages/user-realname/user-realname'}})">
				<button class="btn">立即实名</button>
			</view>
			<view class="center color9 mgb10" @click="Router.navigateTo({route:{path:'/pages/personHome/personHome'}})">下次验证</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show:false,
				is_realnameshow:false,
				mainData:{}
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			prev(){
				const self = this;
				self.$Router.back(1)
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			realnameshow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_realnameshow = !self.is_realnameshow
			},
			
			showToast(){
				const self = this;
				self.$Utils.showToast('实名认证已通过', 'none', 1000)
			},
		}
	};
</script>

<style>
	
	@import "../../assets/style/editInfor.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.userHead{height: 460rpx;background-image: linear-gradient(to bottom, #222222, #4e4e4e);}
	.userHead .arrowR{width: 18rpx;height: 32rpx;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;padding: 4rpx;border:1px solid #F5F5F5}
	.userLable{line-height: 40rpx;border-radius: 8rpx;background: #fff;padding: 0 10rpx;}
	
	.myRowBetween{margin: -70rpx 4% 10rpx 4%;}
	.myRowBetween .item{padding: 30rpx 4%;}
	
	.realnameshow{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 42;}
	.realnameshow .bjImg{width: 100%;display: block;}
</style>
