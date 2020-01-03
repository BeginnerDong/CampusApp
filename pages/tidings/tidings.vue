<template>
	<view>
		
		<view class="flexRowBetween indexTit borderB1 W-Fixed">
			<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})"><image src="../../static/images/the-message-img.png" mode=""></image></view>
			<view class="fs16 color6">消息</view>
			<view></view>
		</view>
		
		<view class="orderNav flexRowBetween whiteBj color6 W-Fixed borderB1" style="top: 98rpx;">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">赞</view>
			<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">评论</view>
			<view class="tt flexCenter" :class="curr==3?'on':''" @click="changeCurr('3')">私信</view>
			<view class="tt flexCenter" :class="curr==4?'on':''" @click="changeCurr('4')">系统</view>
		</view>
		
		<view class="pdtb25"></view>
		<view class="pdtb20"></view>
		<view class="f5H5"></view>
		
		<view class="zanCont" v-show="curr==1">
			<view class="item" v-for="(item,index) in zanData" :key="index">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-img.png" mode=""></image></view>
					<view class="name mgl10">
						<view class="fs15">竹蜻蜓</view>
						<view class="fs10 color9">12-25 16:31</view>
					</view>
				</view>
				<view class="pdtb10">赞了这条活动</view>
				<view class="cont f5bj flex">
					<view class="pic"><image src="../../static/images/the-message-img1.png" mode=""></image></view>
					<view class="rr">
						<view class="tit fs13 avoidOverflow">标题标题标题标题标题标题标题标题</view>
						<view class="text avoidOverflow2 fs12 color6">双菇还记得老汉还说加工费几点过来价格付款落地生根加工费回户加工费点了结果火锅店机构管理很反感华海景房</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="zanCont" v-show="curr==2">
			<view class="item" v-for="(item,index) in zanData" :key="index">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-img.png" mode=""></image></view>
					<view class="name mgl10">
						<view class="fs15">竹蜻蜓</view>
						<view class="fs10 color9">12-31 11:31</view>
					</view>
				</view>
				<view class="pdtb10">真心不错，服务态度也很好，已经推荐给朋友，很愉快的一次购物</view>
				<view class="cont f5bj flex">
					<view class="pic"><image src="../../static/images/the-message-img1.png" mode=""></image></view>
					<view class="rr">
						<view class="tit fs13 avoidOverflow">标题标题标题标题标题标题标题标题</view>
						<view class="text avoidOverflow2 fs12 color6">双菇还记得老汉还说加工费几点过来价格付款落地生根加工费回户加工费点了结果火锅店机构管理很反感华海景房</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="sixinCont" v-show="curr==3">
			<view class="item flex"  v-for="(item,index) in sixinData" :key="index" @click="Router.navigateTo({route:{path:'/pages/userHome-tidings/userHome-tidings'}})">
				<view class="ll">
					<image src="../../static/images/home-img.png" mode=""></image>
				</view>
				<view class="rr">
					<view class="flexRowBetween pdb5">
						<view class="fs15">竹蜻蜓</view>
						<view class="fs10 color9">12-31</view>
					</view>
					<view class="fs12 color6 avoidOverflow">国际法大使馆和公交卡粉红色的刚回来看好厉害公交卡</view>
				</view>
			</view>
		</view>
		
		<view class="dialogBox pdlr4 f5bj" v-show="curr==4" >
			<view class="item left">
				<view class="time center fs13 color9">19-12-27 14:23</view>
				<view class="infor">
					<image class="Photo" src="../../static/images/xiaoxi-img.png" >
					<view class="text">你好，想和你详细沟通下，现在方便吗？非的行列读后感 割发代首家里给会返利是这个和磊哥换个方式的尖括号割发代首开了个会了过节费肯定是联合国干活附近的干活附近的看更进反馈多少个一缸发动机开始估红富士康</view>
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
					<image src="../../static/images/nabar5-a.png" />
				</view>
				<view class="text this-text">消息</view>
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
				zanData:[{},{},{},{}],
				sixinData:[{},{},{},{}]
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
	@import "../../assets/style/tidings.css";
	@import "../../assets/style/zanCont.css";
	
	page{padding-bottom: 140rpx;}
	.orderNav .tt{width: 25%;}
	.orderNav .tt.on:after{height: 0;}
	
	.sixinCont .item{padding: 30rpx 4%;border-bottom: 1px solid #eee;}
	.sixinCont .ll{width: 18%}
	.sixinCont .ll image{width: 100rpx;height: 100rpx;border-radius: 50%; display: block;}
	.sixinCont .rr{width: 82%;}
	.dialogBox{position: fixed;left: 0;top: 180rpx;right: 0;bottom: 110rpx;overflow-y: auto;padding-bottom: 60rpx;box-sizing: border-box;}
	
</style>
