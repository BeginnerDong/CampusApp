<template>
	<view>
		
		<view class="userHead white">
			<view class="headBJ"><image src="../../static/images/gerenzhuye-img.png" mode=""></image></view>
			<view class="pr" style="z-index: 2;">
				<view class="headTit flexRowBetween pdlr4 pdt15 pdb10 white">
					<view class="item" @click="prev()"><image class="arrowR" style="margin-left: 0;" src="../../static/images/arrowL.png" mode=""></image></view>
					<view class="item fs15 center">个人主页</view>
					<view class="item fs12 flexEnd" @click="Router.navigateTo({route:{path:'/pages/personHome-replaceImg/personHome-replaceImg'}})">更换背景图</view>
				</view>
				<view class="pdt30 pdlr4 flex">
					<view @click="Router.navigateTo({route:{path:'/pages/personHome-replaceImg/personHome-replaceImg'}})"><image class="photo" src="../../static/images/about-img.png" mode=""></image></view>
					<view style="width: 70%;">
						<view class="fs16"></view>
					</view>
				</view>
			</view>
		</view>
		<view class="whiteBj pdlr4 fs13 center" style="height: 100rpx;">
			<view class="radius10 boxShaow flexRowBetween navTwoBox">
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/person-followok/person-followok'}})">
					<view><image class="icon" src="../../static/images/gerenzhuye-icon.png" mode=""></image></view>
					<view>关注</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/person-follow/person-follow'}})">
					<view><image class="icon" src="../../static/images/gerenzhuye-icon1.png" mode=""></image></view>
					<view>被关注</view>
				</view>
			</view>
		</view>
		<view class="myRowBetween whiteBj">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/person-name/person-name'}})">
				<view class="ll">昵称</view>
				<view class="rr">
					<text>雨果果</text>
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/person-autograph/person-autograph'}})">
				<view class="ll">签名</view>
				<view class="rr">
					<text class="color6">输入签名</text>
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/person-role/person-role'}})">
				<view class="ll">角色</view>
				<view class="rr">
					<text>学生·在读</text>
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/person-personInfor/person-personInfor'}})">
				<view class="ll">个人资料</view>
				<view class="rr">
					<text class="color6">查看</text>
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="mglr4 userHomeLis mgt15">
			<view class="item flexRowBetween" v-for="(item,index) in userHomeDate" :key="index">
				<view class="data">
					<view class="year">2018</view>
					<view class="day">12/27</view>
				</view>
				<view class="infor">
					<view class="list flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
						<view class="pic"><image src="../../static/images/gerenzhuye-img1.png" mode=""></image></view>
						<view class="rr">
							<view class="pdt5 pdb5 avoidOverflow">我在国外做博后</view>
							<view class="fs12 color6 avoidOverflow2">刚发的还是个加工费单身公害感觉粉色看看经历过价格付款的刚发的国防生的刚</view>
						</view>
					</view>
					<view class="list flexRowBetween"  @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
						<view class="pic"><image src="../../static/images/home-img2.png" mode=""></image></view>
						<view class="rr">
							<view class="pdt5 pdb5 avoidOverflow">我在国外做博后</view>
							<view class="fs12 color6 avoidOverflow2">刚发的还是个加工费单身公害感觉粉色看看经历过价格付款的刚发的国防生的刚</view>
						</view>
					</view>
				</view>
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
				score:'',
				wx_info:{},
				userHomeDate:[{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			prev(){
				const self = this;
				self.$router.go(-1)
			}
		}
	};
</script>

<style>
	@import "../../assets/style/userHome.css";
	@import "../../assets/style/editInfor.css";
	
	page{background: #F5F5F5;}
	.userHead{height: 400rpx;}
	.navTwoBox{background: #fff;border-radius: 10rpx;padding: 30rpx 0 20rpx 0;position: relative;top: -40rpx;}
	.navTwoBox .item{width: 50%;box-sizing: border-box;position: relative;}
	.navTwoBox .item.one:after{content: '';width: 1px;height: 30rpx;background: #F5F5F5;position: absolute;top: 50%;right: 0;transform: translateY(-50%);}
	.navTwoBox .item .icon{width: 36rpx;height: 36rpx;display: block;margin:0 auto 10rpx auto;}
	
	.myRowBetween .item{padding: 30rpx 4%;}
	.myRowBetween .item .rr{font-size: 24rpx;}
</style>
