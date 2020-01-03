<template>
	<view>
		
		<view class="comment mglr4 activeBox">
			<view class="child">
				<image class="FX-icon" src="../../static/images/activity-icon1.png" mode=""></image>
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="gzBtn fs12 center white color6Bj">关注</view>
					</view>
				</view>
				<view class="ftw pdt10 pdb5">参观大唐芙蓉园</view>
				<view class="fs12 color6">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了规划设计法海跟is干活换个飞机多少个扩大复合弓鹤骨鸡肤大好时光</view>
				<view class="imgbox">
					<view class="img lisThree">
						<image src="../../static/images/the-order-img.png" mode=""></image>
					</view>
					<view class="img lisThree">
						<image src="../../static/images/the-order-img.png" mode=""></image>
					</view>
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
		<view class="f5H5"></view>
		<view class="pdlr4 pdb10">
			<view class="ftw pdt15 pdb10">参与条件</view>
			<view class="fs12 color6 xqInfor">
				<view>1、回复健康的撒花四氯化硅鹤骨鸡肤商户管理挂号费世界观和规划局分是价格付款苏广加了加工费老岁接个了加工费克鲁赛德就过来监管科廊坊市寄过来</view>
				<view>2、挂号费了撒谎过份了和估计富商大贾和更进反馈单身公害挂号费就开始干活规划局分开始的话购房人几十个热干活</view>
				<view>3、刚发的广告费九十点了光和热和鬼斧神工和公交卡</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="pdlr4">
			<view class="flexRowBetween pdtb15 borderB1">
				<view class="fs14">开始时间</view>
				<view class="fs13">2019年12月12日8点</view>
			</view>
			<view class="flexRowBetween pdtb15">
				<view class="fs14">活动地址</view>
				<view class="fs13">西安市长安区韦曲街道</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="pdtb25 mglr4">
			<view class="submitbtn">
				<button class="btn" type="button"  @click="alertBoxShow">报名参加</button>
			</view>
			<view class="center fs13 color6 pdtb15">报名成功，可以在我的-参加活动中查看入场券</view>
		</view>
		
		
		<!-- 报名提示弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="alertBox center pdt20 radius10 whiteBj" v-show="is_alertBox">
			<view>提示：</view>
			<view class="pdt10 pdb20 color6 fs13">确定参加此次活动？</view>
			<view class="flexRowBetween selt seltBtn">
				<view class="half" @click="alertBoxShow">取消</view>
				<view class="half" @click="signUpOKShow">确定</view>
			</view>
		</view>
		
		<view class="signUpOK radius10 whiteBj" v-show="is_signUpOK">
			<view class="closeBtn" @click="Router.redirectTo({route:{path:'/pages/activity/activity'}})">×</view>
			<view class="center pdb20">报名成功！</view>
			<view class="quanCard fs12">
				<view class="center fs16 quanTit pdb10">入场券</view>
				<view class="">时间：2019.12.26 18:30</view>
				<view>地点：体育馆</view>
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
				is_alertBox:false,
				is_signUpOK:false
				
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			alertBoxShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_alertBox = !self.is_alertBox
			},
			signUpOKShow(){
				const self = this;
				self.is_alertBox = !self.is_alertBox
				self.is_signUpOK = !self.is_signUpOK
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
	@import "../../assets/style/alertBox.css";
	.comment .child{border-bottom: 0;}
	.xqInfor view{margin-bottom: 10rpx;}
	
	/* 报名成功弹框 */
	.signUpOK{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 60;box-sizing: border-box;padding: 50rpx 30rpx 120rpx 30rpx;}
	
	
	.quanCard{width: 100%;height: 220rpx;padding: 30rpx 30rpx 30rpx 190rpx;background: url(../../static/images/canyuhuodong-img.png) no-repeat 0 0/100% 100%;box-sizing: border-box;}
</style>
