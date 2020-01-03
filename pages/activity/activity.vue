<template>
	<view>
		
		<view class="flexRowBetween indexTit borderB1 whiteBj W-Fixed">
			<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})"><image src="../../static/images/the-message-img.png" mode=""></image></view>
			<view class="fs16 color6 flexRowBetween">活动</view>
			<view @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})"><image class="seachBtn" src="../../static/images/home-icon.png" mode=""></image></view>
		</view>
		
		
		<view class="orderNav flexRowBetween whiteBj color6 W-Fixed borderB1" style="top: 98rpx;">
			<view class="tt flexCenter" :class="is_zongheShow==true?'on':''" @click="zongheShow">城市<image class="arrowB" src="../../static/images/activity-icon.png" mode=""></image></view>
			<view class="tt flexCenter" :class="is_time==true?'on':''" @click="timeShow">时间<image class="arrowB" src="../../static/images/activity-icon.png" mode=""></image></view>
			<view class="tt flexCenter" :class="is_screenShow==true?'on':''"  @click="screenShow">筛选<image class="arrowB" src="../../static/images/activity-icon.png" mode=""></image></view>
		</view>
		<view class="pdtb25"></view>
		<view class="pdtb20"></view>
		
		<view class="black-bj" v-show="is_show"></view>
		<!-- 城市 -->
		<view class="zongheShow whiteBj fs13" v-show="is_zongheShow">
			<view class="item flexRowBetween" v-for="(item,index) in cityData" :key="index" :class="num==index?'on':''" @click="changeNum(index)">
				<view class="tt">{{item}}</view>
				<image class="selIcon" :src="num==index?'../../static/images/xiaoxi-icon.png':''" mode=""></image>
			</view>
		</view>
		
		<!-- 时间 -->
		<view class="zongheShow whiteBj fs13" v-show="is_timeShow">
			<view class="item flexRowBetween" v-for="(item,index) in timeData" :key="index" :class="numCurr==index?'on':''" @click="changeTime(index)">
				<view class="tt">{{item}}</view>
				<image class="selIcon" :src="numCurr==index?'../../static/images/xiaoxi-icon.png':''" mode=""></image>
			</view>
		</view>
		<!-- 筛选 -->
		<view class="zongheShow whiteBj fs13" v-show="is_screenShow">
			<view class="item flexRowBetween" v-for="(item,index) in screenData" :key="index" :class="screenCurr==index?'on':''" @click="changeScreen(index)">
				<view class="tt">{{item}}</view>
				<image class="selIcon" :src="screenCurr==index?'../../static/images/xiaoxi-icon.png':''" mode=""></image>
			</view>
		</view>
		
		
		<view class="fabubtn" @click="Router.navigateTo({route:{path:'/pages/activityFaBu/activityFaBu'}})"><image class="icon" src="../../static/images/home-icon1.png" mode=""></image></view>
		
		<view class="comment mglr4 mgt15 activeBox">
			<view class="child pr oh">
				<image class="FX-icon" src="../../static/images/activity-icon1.png" mode=""></image>
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="gzBtn fs12 center white">关注</view>
					</view>
				</view>
				<view class="ftw pdt10 pdb5">参观大唐芙蓉园</view>
				<view class="fs12 color6" @click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail'}})">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了规划设计法海跟is干活换个飞机多少个扩大复合弓鹤骨鸡肤大好时光</view>
				<view class="imgbox">
					<view class="img lisThree">
						<image src="../../static/images/the-order-img.png" mode=""></image>
					</view>
					<view class="img lisThree">
						<image src="../../static/images/the-order-img.png" mode=""></image>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs12">
					<view class="lis flex">
						<image src="../../static/images/activity-icon2.png" mode=""></image>
						<view>收藏</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon5.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
			<view class="child pr oh">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="gzBtn fs12 center white">关注</view>
					</view>
				</view>
				<view class="ftw pdt10 pdb5">参观大唐芙蓉园</view>
				<view class="fs12 color6" @click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail'}})">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了规划设计法海跟is干活换个飞机多少个扩大复合弓鹤骨鸡肤大好时光</view>
				<view class="label pdt15 flexEnd fs12">
					<view class="lis flex">
						<image src="../../static/images/activity-icon2.png" mode=""></image>
						<view>收藏</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon5.png" mode=""></image>
						<view>1562</view>
					</view>
				</view>
			</view>
			<view class="child pr oh">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="gzBtn fs12 center white">关注</view>
					</view>
				</view>
				<view class="ftw pdt10 pdb5">参观大唐芙蓉园</view>
				<view class="fs12 color6" @click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail'}})">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了规划设计法海跟is干活换个飞机多少个扩大复合弓鹤骨鸡肤大好时光</view>
				<view class="label pdt15 flexEnd fs12">
					<view class="lis flex">
						<image src="../../static/images/activity-icon2.png" mode=""></image>
						<view>收藏</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon5.png" mode=""></image>
						<view>1562</view>
					</view>
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
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">活动</view>
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
				num:0,
				is_zongheShow:false,
				cityData:['北京','上海','深圳','杭州','广东','陕西'],
				numCurr:0,
				is_timeShow:false,
				timeData:['最近三天','最近一周','最近一个月','最近半年','最近一年'],
				screenCurr:0,
				is_screenShow:false,
				screenData:['热门','新鲜']
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeNum(index){
				const self = this;
				self.num = index
				self.is_show = !self.is_show
				self.is_zongheShow = !self.is_zongheShow
			},
			zongheShow(){
				const self = this;
				self.is_show = true;
				self.is_zongheShow = !self.is_zongheShow;
				self.is_timeShow = false;
				self.is_screenShow = false
			},
			changeTime(index){
				const self = this;
				self.numCurr = index
				self.is_show = !self.is_show
				self.is_timeShow = !self.is_timeShow
			},
			timeShow(){
				const self = this;
				self.is_show = true;
				self.is_timeShow = !self.is_timeShow;
				self.is_zongheShow = false;
				self.is_screenShow = false
			},
			changeScreen(index){
				const self = this;
				self.screenCurr = index
				self.is_show = !self.is_show
				self.is_screenShow = !self.is_screenShow
			},
			screenShow(){
				const self = this;
				self.is_show = true;
				self.is_screenShow = !self.is_screenShow
				self.is_zongheShow = false;
				self.is_timeShow = false;
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/comment.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	.orderNav .tt.on::after{height: 0;}
	
	
	.comment .child{padding: 30rpx 4%;background: #FFF; border-bottom: 0;margin-bottom: 30rpx;border-radius: 10rpx;}
	.black-bj, .black-bj2{top: 180rpx;}
</style>
