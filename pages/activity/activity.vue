<template>
	<view>
		<view style="padding-top: 44px;" class="W-Fixed">
			<view class="flexRowBetween indexTit borderB1 whiteBj " >
				<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})">
					<image :src="userInfoData.mainImg&&userInfoData.mainImg.length>0?userInfoData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
				</view>
				<view class="fs16 color6 flexRowBetween">活动</view>
				<view @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})"><image class="seachBtn" src="../../static/images/home-icon.png" mode=""></image></view>
			</view>
			<view class="orderNav flexRowBetween whiteBj color6  borderB1">
				<view class="tt flexCenter" :class="is_zongheShow==true?'on':''" @click="zongheShow">城市<image class="arrowB" src="../../static/images/activity-icon.png" mode=""></image></view>
				<view class="tt flexCenter" :class="is_timeShow==true?'on':''" @click="timeShow">时间<image class="arrowB" src="../../static/images/activity-icon.png" mode=""></image></view>
				<view class="tt flexCenter" :class="is_screenShow==true?'on':''"  @click="screenShow">筛选<image class="arrowB" src="../../static/images/activity-icon.png" mode=""></image></view>
			</view>
		</view>
		
		<view style="height: 83px;background-color: #f5f5f5;"></view>
		
		
		<view class="pdtb25"></view>
		
		<view class="black-bj" v-if="is_show"></view>
		<!-- 城市 -->
		<view class="zongheShow whiteBj fs13" v-if="is_zongheShow">
			<view class="item flexRowBetween" v-for="(item,index) in cityData" :key="index" :class="cityIndex==index?'on':''" 
			@click="changeCity(index)">
				<view class="tt">{{item}}</view>
				<image class="selIcon" :src="cityIndex==index?'../../static/images/xiaoxi-icon.png':''" mode=""></image>
			</view>
		</view>
		
		<!-- 时间 -->
		<view class="zongheShow whiteBj fs13" v-if="is_timeShow">
			<view class="item flexRowBetween" v-for="(item,index) in timeData" :key="index" :class="timeIndex==index?'on':''" 
			@click="changeTime(index)">
				<view class="tt">{{item.title}}</view>
				<image class="selIcon" :src="timeIndex==index?'../../static/images/xiaoxi-icon.png':''" mode=""></image>
			</view>
		</view>
		<!-- 筛选 -->
		<view class="zongheShow whiteBj fs13" v-if="is_screenShow">
			<view class="item flexRowBetween" v-for="(item,index) in screenData" :key="index" :class="screenIndex==index?'on':''" 
			@click="changeScreen(index)">
				<view class="tt">{{item}}</view>
				<image class="selIcon" :src="screenIndex==index?'../../static/images/xiaoxi-icon.png':''" mode=""></image>
			</view>
		</view>
		
		
		<view class="fabubtn" v-if="userInfoData.check_status==2"  @click="Router.navigateTo({route:{path:'/pages/activityFaBu/activityFaBu'}})"><image class="icon" src="../../static/images/home-icon1.png" mode=""></image></view>
		<view class="fabubtn" v-if="userInfoData.check_status!=2" @click="realnameshow"><image class="icon" src="../../static/images/home-icon1.png" mode=""></image></view>
		<view class="comment mglr4 mgt15 activeBox" v-if="mainData.length>0" >
			<view class="child pr oh" v-for="(item,index) in mainData">
				<image v-if="item.enrollMe.length>0" class="FX-icon" src="../../static/images/activity-icon1.png" mode=""></image>
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" :data-user_no ="item.user_no"
						@click="Router.navigateTo({route:{path:'/pages/userHome/userHome?user_no='+$event.currentTarget.dataset.user_no}})">
							<image :src="item.headImg&&item.headImg[0]?item.headImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view class="name">
							<view class="fs12">{{item.name}}</view>
							<view class="fs10 color6">{{item.create_time}}</view>
						</view>
					</view>
					<!-- <view class="flexEnd">
						<view class="gzBtn fs12 center white">关注</view>
					</view> -->
				</view>
				<view class="ftw pdt10 pdb5">{{item.title}}</view>
				<view class="fs12 color6" :data-id="item.id" 
				@click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail?id='+$event.currentTarget.dataset.id}})">
				{{item.content}}
				</view>
				<view class="imgbox" style="padding-top: 20rpx;">
					<view class="img" v-for="(c_item,c_index) in item.mainImg" :class="item.mainImg.length==1?'lisOne':(item.mainImg.length==2?'lisTwo':'lisThree')">
						<image :src="c_item.url" mode="aspectFill" @click="previewImage(index,c_index)"></image>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs12">
					<view class="lis flex" v-if="userInfoData.check_status==2"  @click="clickCollect(index)">
						<image :src="item.collectMe.length>0&&item.collectMe[0].status==1?'../../static/images/activity-icon3.png':'../../static/images/activity-icon2.png'" mode=""></image>
						
						<view>收藏</view>
					</view>
					<view class="lis flex" v-if="userInfoData.check_status!=2" @click="realnameshow">
						<image :src="item.collectMe.length>0&&item.collectMe[0].status==1?'../../static/images/activity-icon3.png':'../../static/images/activity-icon2.png'" mode=""></image>
						
						<view>收藏</view>
					</view>
					<view class="lis flex" v-if="userInfoData.check_status==2" @click="clickGood(index)">
						<image :src="item.goodMe.length>0&&item.goodMe[0].status==1?'../../static/images/home-icon6.png':'../../static/images/home-icon5.png'" mode=""></image>
						<view>{{item.good?item.good.count:0}}</view>
					</view>
					
					<view class="lis flex" v-if="userInfoData.check_status!=2" @click="realnameshow">
						<image :src="item.goodMe.length>0&&item.goodMe[0].status==1?'../../static/images/home-icon6.png':'../../static/images/home-icon5.png'" mode=""></image>
						<view>{{item.good?item.good.count:0}}</view>
					</view>
				</view>
			</view>
		</view>
		<view v-else>
			<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
		</view>
		
		<view class="realnameshow whiteBj radius10 pdb25" v-if="is_realnameshow">
			<view class="closebtn" style="z-index: 2;" @click="realnameshow">×</view>
			<view>
				<image class="bjImg w" src="../../static/images/about-icon2.png" mode="widthFix"></image>
			</view>
			<view class="pdt25 pdb15 center fs18 ftw">实名认证</view>
			<view class="fs12 pdl20 pdr20">您还未认证身份信息，为方便您使用与享受更多的特权服务，请前去认证身份，完善资料。</view>
			<view class="submitbtn mgt25 mgb25" @click="Router.navigateTo({route:{path:'/pages/user-realname/user-realname'}})">
				<button class="btn">立即实名</button>
			</view>
			<view class="center color9 mgb10" @click="realnameshow">下次验证</view>
			
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
				cityIndex:-1,
				is_zongheShow:false,
				cityData:['西安市','咸阳市','渭南市','铜川市','宝鸡市','汉中市','安康市'],
				timeIndex:-1,
				is_timeShow:false,
				is_time:false,
				timeData:[{title:'最近三天',value:86400*2},{title:'最近一周',value:86400*6},{title:'最近一个月',value:86400*30},{title:'最近半年',value:86400*182},{title:'最近一年',value:86400*364}],
				screenIndex:-1,
				is_screenShow:false,
				screenData:['热门','新鲜'],
				searchItem:{
					thirdapp_id:2,
					report:0,
					type:2,
					user_type:0
				},
				mainData:[],
				userInfoData:{},
				is_realnameshow:false,
				order:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		onPullDownRefresh() {
			const self = this;
			console.log('refresh');
			self.searchItem={
				thirdapp_id:2,
				report:0,
				type:2,
				user_type:0
			};
			self.order = {};
			self.getMainData(true)	
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
			
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				if(JSON.stringify(self.order)!="{}"){
					postData.order = self.$Utils.cloneForm(self.order);
				};
				
				postData.getAfter = {
					enrollMe: {
						tableName: 'Log',
						searchItem: {
							status:['in',[1]],
							type:7,
							user_no:wx.getStorageSync('user_info').user_no
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
					goodMe: {
						tableName: 'Log',
						searchItem: {
							status:['in',[1,-1]],
							type:1,
							user_no:wx.getStorageSync('user_info').user_no
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
					good: {
						tableName: 'Log',
						searchItem: {
							status:1,
							type:1,
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:1
						    }
						  ]
						},
					},
					
					collect: {
						tableName: 'Log',
						searchItem: {
							status:1,
							type:2,
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:2
						    }
						  ]
						},
					},
					collectMe: {
						tableName: 'Log',
						searchItem: {
							status:['in',[1,-1]],
							type:2,
							user_no:wx.getStorageSync('user_info').user_no
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					uni.stopPullDownRefresh();
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.newsGet(postData, callback);
			},
			
			clickGood(index) {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (self.mainData[index].goodMe.length == 0) {
					self.addGoodLog(index)
				} else {
					self.updateGoodLog(index)
				};
			},
			
			addGoodLog(index) {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '点赞成功',
					relation_id: self.mainData[index].id,
					relation_table:'News',
					user_no: uni.getStorageSync('user_info').user_no,
					relation_user:self.mainData[index].user_no
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData[index].goodMe.push({
							status: 1,
							id: res.info.id
						});
						self.mainData[index].good.count = self.mainData[index].good.count+1
						//self.$Utils.showToast('已收藏', 'none', 1000)
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateGoodLog(index) {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.mainData[index].goodMe[0].id
						
					},
					data: {
						status: -self.mainData[index].goodMe[0].status
					}
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData[index].goodMe[0].status = -self.mainData[index].goodMe[0].status;
						if(self.mainData[index].goodMe[0].status==1){
							self.mainData[index].good.count = self.mainData[index].good.count+1
							//self.$Utils.showToast('已收藏', 'none', 1000)
						}else{
							self.mainData[index].good.count = self.mainData[index].good.count-1
							self.$Utils.showToast('取消成功', 'none', 1000)
						}
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},
			
			clickCollect(index) {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (self.mainData[index].collectMe.length == 0) {
					self.addCollectLog(index)
				} else {
					self.updateCollectLog(index)
				};
			},
			
			addCollectLog(index) {
				const self = this;
				const postData = {};
				postData.data = {
					type: 2,
					title: '收藏成功',
					relation_id: self.mainData[index].id,
					relation_table:'News',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData[index].collectMe.push({
							status: 1,
							id: res.info.id
						});
						self.mainData[index].collect.count = self.mainData[index].collect.count+1
						//self.$Utils.showToast('已收藏', 'none', 1000)
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateCollectLog(index) {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.mainData[index].collectMe[0].id
						
					},
					data: {
						status: -self.mainData[index].collectMe[0].status
					}
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData[index].collectMe[0].status = -self.mainData[index].collectMe[0].status;
						if(self.mainData[index].collectMe[0].status==1){
							self.mainData[index].collect.count = self.mainData[index].collect.count+1
							//self.$Utils.showToast('已收藏', 'none', 1000)
						}else{
							self.mainData[index].collect.count = self.mainData[index].collect.count-1
							self.$Utils.showToast('取消成功', 'none', 1000)
						}
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},
			
			realnameshow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_realnameshow = !self.is_realnameshow
			},
			
			changeCity(index){
				const self = this;
				self.cityIndex = index;
				self.searchItem.city = self.cityData[self.cityIndex];
				self.is_show = !self.is_show
				self.is_zongheShow = !self.is_zongheShow;
				self.getMainData(true)
			},
			
			zongheShow(){
				const self = this;
				self.is_zongheShow = !self.is_zongheShow;
				if(!self.is_zongheShow){
					self.is_show = false;
				}else{
					self.is_show = true;
				}
				self.is_timeShow = false;
				self.is_screenShow = false
			},
			
			changeTime(index){
				const self = this;
				var nowTime = Date.parse(new Date())/1000;
				self.timeIndex = index;
				self.searchItem.create_time = ['>',nowTime-self.timeData[self.timeIndex].value];
				self.is_show = !self.is_show;
				self.is_timeShow = !self.is_timeShow;
				self.getMainData(true)
			},
			
			timeShow(){
				const self = this;
			
				self.is_timeShow = !self.is_timeShow;
				if(!self.is_timeShow){
					self.is_show = false;
				}else{
					self.is_show = true;
				}
				self.is_zongheShow = false;
				self.is_screenShow = false
			},
			
			changeScreen(index){
				const self = this;
				self.screenIndex = index;
				if(self.screenData[self.screenIndex]=='热门'){
					self.order = {
						favor:'desc'
					}
				}else{
					self.order = {}
				};
				self.is_show = !self.is_show
				self.is_screenShow = !self.is_screenShow;
				self.getMainData(true)
			},
			
			
			screenShow(){
				const self = this;
				self.is_screenShow = !self.is_screenShow
				if(!self.is_screenShow){
					self.is_show = false;
				}else{
					self.is_show = true;
				}
				self.is_zongheShow = false;
				self.is_timeShow = false;
			},
			
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
	.realnameshow{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 42;}
	.realnameshow .bjImg{width: 100%;display: block;}
</style>
