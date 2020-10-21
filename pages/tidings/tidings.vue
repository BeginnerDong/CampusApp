<template>
	<view>
		<view style="padding-top: 44px;" class="W-Fixed">
			<view class="flexRowBetween indexTit borderB1">
				<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})">
					<image :src="userInfoData.mainImg&&userInfoData.mainImg.length>0?userInfoData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
				</view>
				<view class="fs16 color6">消息</view>
				<view></view>
			</view>
			
			<view class="orderNav flexRowBetween whiteBj color6  borderB1" style="top: 98rpx;">
				<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">赞</view>
				<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">评论</view>
				<view class="tt flexCenter" :class="curr==3?'on':''" @click="changeCurr('3')">私信</view>
				<view class="tt flexCenter" :class="curr==4?'on':''" @click="changeCurr('4')">系统</view>
			</view>
		</view>
		<view style="height: 83px;background-color: #f5f5f5;"></view>
		
		<view class="pdtb25"></view>
		
		<view class="f5H5"></view>
		<view v-show="curr==1">
			<view class="zanCont" v-if="goodData.length>0">
				<view class="item" v-for="(item,index) in goodData" :key="index">
					<view class="flex">
						<view class="photo">
							<image style="border-radius: 50%;" :src="item.userinfo&&item.userinfo[0]&&item.userinfo[0].mainImg&&
							item.userinfo[0].mainImg[0]?item.userinfo[0].mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view class="name mgl10">
							<view class="fs15">{{item.userinfo&&item.userinfo[0]&&item.userinfo[0]?item.userinfo[0].name:''}}</view>
							<view class="fs10 color9">{{item.create_time}}</view>
						</view>
					</view>
					<view class="pdtb10">赞了这条{{item.news&&item.news[0]&&item.news[0].type==1?'动态':(item.news&&item.news[0]&&item.news[0].type==2?'活动':'社区文章')}}</view>
					<view class="cont f5bj flex" @click="toDetail(item.news[0].type,item.news[0].id)">
						<view class="pic" v-if="item.news&&item.news[0]&&item.news[0].mainImg
						&&item.news[0].mainImg.length>0">
						<image  :src="item.news&&item.news[0]&&item.news[0].mainImg&&
						item.news[0].mainImg[0]?item.news[0].mainImg[0].url:''" mode=""></image>
						</view>
						<view class="rr">
							<view class="tit fs13 avoidOverflow">{{item.news&&item.news[0]&&item.news[0]?item.news[0].title:''}}</view>
							<view class="text avoidOverflow2 fs12 color6">{{item.news&&item.news[0]&&item.news[0]?item.news[0].content:''}}</view>
						</view>
					</view>
				</view>
			</view>
			<view v-else>
				<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
			</view>
		</view>
		
		<view v-show="curr==2">
			<view class="zanCont" v-if="replyData.length>0">
				<view class="item" v-for="(item,index) in replyData" :key="index">
					<view class="flex">
						<view class="photo"><image style="border-radius: 50%;" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image></view>
						<view class="name mgl10">
							<view class="fs15">{{item.title}}</view>
							<view class="fs10 color9">{{item.create_time}}</view>
						</view>
					</view>
					<view class="pdtb10" >{{item.content}}</view>
					<view class="cont f5bj flex" @click="toDetail(item.news[0].type,item.news[0].id)">
						<view class="pic" v-if="item.news&&item.news[0]&&item.news[0].mainImg
						&&item.news[0].mainImg.length>0">
						<image :src="item.news&&item.news[0]&&item.news[0].mainImg&&
						item.news[0].mainImg[0]?item.news[0].mainImg[0].url:''" mode=""></image>
						</view>
						<view class="rr">
							<view class="tit fs13 avoidOverflow">{{item.news&&item.news[0]&&item.news[0]?item.news[0].title:''}}</view>
							<view class="text avoidOverflow2 fs12 color6">{{item.news&&item.news[0]&&item.news[0]?item.news[0].content:''}}</view>
						</view>
					</view>
				</view>
			</view>
			<view v-else>
				<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
			</view>
		</view>
		
		<view v-show="curr==3">
			<view class="sixinCont" v-if="messageData.length>0">
				<view class="item flex"  v-for="(item,index) in messageData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/userHome-tidings/userHome-tidings?id='+$event.currentTarget.dataset.id}})">
					<view class="ll">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
					</view>
					<view class="rr">
						<view class="flexRowBetween pdb5">
							<view class="fs15">{{item.name!=''?item.name:'用户'+item.user_no}}</view>
							<view class="fs10 color9">{{item.update_time}}</view>
						</view>
						<view class="fs12 color6 avoidOverflow">{{item.last_chat?item.last_chat.content:""}}</view>
					</view>
				</view>
			</view>
			<view v-else>
				<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
			</view>
		</view>
		
		<view v-show="curr==4">
			<view class="dialogBox pdlr4 f5bj" v-if="systemData.length>0" >
				<view class="item left" v-for="item in systemData">
					<view class="time center fs13 color9">{{item.create_time}}</view>
					<view class="infor">
						<image class="Photo" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:'../../static/images/about-img.png'" >
						<view class="text">{{item.content}}</view>
					</view>
				</view>
			</view>
			<view v-else>
				<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
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
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/activity/activity'}})">
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
			<view class="navbar_item">
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
				sixinData:[{},{},{},{}],
				goodData:[],
				systemData:[],
				replyData:[],
				messageData:[],
				userInfoData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getGoodData','getUserInfoData'], self);
		},
		
		methods: {
			
			toDetail(type,id){
				const self = this;
				console.log(type)
				console.log(id)
				if(type==2){
					self.Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail?id='+id}})
				}else{
					self.Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two?id='+id}})
				}
			},
			
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
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr;
					if(self.curr==1){
						self.getGoodData(true)
					}else if(self.curr==2){
						self.getReplyData(true)
					}else if(self.curr==3){
						self.getMessageData(true)
					}else if(self.curr==4){
						self.getSystemData(true)
					}
				}
			},
			
			getGoodData(isNew) {
				const self = this;
				if (isNew) {
					self.goodData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:1,
					relation_user:uni.getStorageSync('user_info').user_no,
					user_type:0,
					relation_table:'News',
					
				};
				postData.tokenFuncName = 'getUserToken';
				postData.getAfter = {
					news:{
						tableName:'News',
						middleKey:'relation_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
					userinfo:{
						tableName:'UserInfo',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.goodData.push.apply(self.goodData,res.info.data)
					}
			
					self.$Utils.finishFunc('getGoodData');
				};
				self.$apis.logGet(postData, callback);
			},
			
			getReplyData(isNew) {
				const self = this;
				if (isNew) {
					self.replyData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:2,
					relation_user:uni.getStorageSync('user_info').user_no,
					user_type:0,
					relation_table:'News',
					passage1:['in',['']]
				};
				postData.tokenFuncName = 'getUserToken';
				postData.getAfter = {
					news:{
						tableName:'News',
						middleKey:'relation_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
					
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.replyData.push.apply(self.replyData,res.info.data)
					}		
					self.$Utils.finishFunc('getReplyData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			
			getMessageData(isNew) {
				const self = this;
				if (isNew) {
					self.messageData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData.push.apply(self.messageData,res.info.data)
					}		
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.getChatList(postData, callback);
			},
			
			getSystemData(isNew) {
				const self = this;
				if (isNew) {
					self.systemData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:1,
					user_no:uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.systemData.push.apply(self.systemData,res.info.data)
					}
					self.$Utils.finishFunc('getSystemData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
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
	.dialogBox{position: fixed;left: 0;top: 190rpx;right: 0;bottom: 110rpx;overflow-y: auto;padding-bottom: 60rpx;box-sizing: border-box;}
	
</style>
