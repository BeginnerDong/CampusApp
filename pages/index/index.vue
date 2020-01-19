<template>
	<view>
		
		<view class="flexRowBetween indexTit borderB1 W-Fixed">
			<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})">
				<image :src="userInfoData.mainImg&&userInfoData.mainImg.length>0?userInfoData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
			</view>
			<view class="titNav fs16 color6 flexRowBetween">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">推荐</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">关注</view>
			</view>
			<view @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})"><image class="seachBtn" src="../../static/images/home-icon.png" mode=""></image></view>
		</view>
		
		<view class="pdtb25"></view>
		
		<view class="fabubtn" v-if="userInfoData.check_status==2" @click="Router.navigateTo({route:{path:'/pages/persDynamic/persDynamic'}})">
			<image class="icon" src="../../static/images/home-icon1.png" mode=""></image>
		</view>
		<view class="fabubtn" v-if="userInfoData.check_status!=2" @click="realnameshow">
			<image class="icon" src="../../static/images/home-icon1.png" mode=""></image>
		</view>
		
		<view class="banner-box pdt15 mglr4">
			<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#222">
				<block v-for="(item,index) in sliderData" :key="index">
					<swiper-item class="swiper-item" :data-url="item.url" @click="Router.navigateTo({route:{path:$event.currentTarget.dataset.url}})">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		
		<view class="comment mglr4 mgt15" v-if="mainData.length>0">
			<view class="child" v-for="(item,index) in mainData" :key="index">
				<image v-if="item.signMe.length>0" class="FX-icon" src="../../static/images/activity-icon1.png" mode=""></image>
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" :data-user_no ="item.user_no"
						@click="Router.navigateTo({route:{path:'/pages/userHome/userHome?user_no='+$event.currentTarget.dataset.user_no}})">
							<image :src="item.headImg&&item.headImg[0]?item.headImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view class="name">
							<view class="fs12">{{item.name}}<span style="font-size: 10pxcolor:#666;">
							</span></view>
							<view class="fs10 color6">{{item.create_time}}</view>
						</view>
					</view>
					<view class="pr" v-if="item.type==3">
						<view class="flexEnd" v-if="userInfoData.check_status==2" @click="shareBtnShow(index)">
							<view class="dian"></view>
							<view class="dian"></view>
							<view class="dian"></view>
						</view>
						
						<view class="flexEnd" v-if="userInfoData.check_status!=2" @click="realnameshow">
							<view class="dian"></view>
							<view class="dian"></view>
							<view class="dian"></view>
						</view>
						<view class="fx-shareBtn fs11 white" v-if="willId==item.id">
							<view class="flexCenter pdb10" @click="clickCollect(index)">
								<image class="icon" :src="item.collectMe.length>0&&item.collectMe[0].status==1?'../../static/images/home-icon9.png':'../../static/images/home-icon7.png'" mode=""></image>
								<text>收藏</text>
							</view>
							<view class="flexCenter" @click="Router.navigateTo({route:{path:'/pages/report/report?id='+willId}})">
								<image class="icon" src="../../static/images/home-icon8.png" mode=""></image>
								<text>举报</text>
							</view>
						</view>
					</view>
					
				</view>
				<view class="" @click="toDetail(item.type,item.id)">
					<view class="ftw pdt10 pdb5" v-if="item.type==2">{{item.title}}</view>
					<view class="fs12" :class="item.type!=2?'pdt10':''">{{item.content}}</view>
					<view class="imgbox" style="padding-top: 20rpx;">
						<view v-for="(c_item,c_index) in item.mainImg" :class="item.mainImg.length==1?'lisOne':(item.mainImg.length==2?'lisTwo':'lisThree')">
							<image :src="c_item.url" mode="aspectFill" @click="previewImage(index,c_index)"></image>
						</view>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex" v-if="item.type==2&&userInfoData.check_status==2" @click="clickCollect(index)">
						<image :src="item.collectMe.length>0&&item.collectMe[0].status==1?'../../static/images/activity-icon3.png':'../../static/images/activity-icon2.png'" mode=""></image>
						
						<view>收藏</view>
					</view>
					<view class="lis flex" v-if="item.type!=1&&userInfoData.check_status!=2" @click="realnameshow">
						<image :src="item.collectMe.length>0&&item.collectMe[0].status==1?'../../static/images/activity-icon3.png':'../../static/images/activity-icon2.png'" mode=""></image>
						
						<view>收藏</view>
					</view>
					<view class="lis flex" v-if="item.type==3">
						<image src="../../static/images/home-icon4.png" mode=""></image>
						<view>{{item.share?item.share.count:0}}</view>
					</view>
					<view class="lis flex" v-if="item.type!=2">
						<image src="../../static/images/home-icon3.png" mode=""></image>
						<view>{{item.comment?item.comment.count:0}}</view>
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
		<view class="black-bj" v-if="is_show"></view>
		
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
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				curr:1,
				is_show:false,
				is_realnameshow:false,
				is_shareBtnShow:false,
				is_like:false,
				userInfoData:{},
				sliderData:[],
				searchItem:{
					thirdapp_id:2,
					report:0,
					user_type:0
				},
				mainData:[],
				willId:-1
				
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
		
		onShow() {
			const self = this;
			self.willId = -1;
		},
		
		methods: {
			
			test() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 3,
					trade_info:'赠送',
					count:1000,
					user_no:uni.getStorageSync('user_info').user_no,
					thirdapp_id:2
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						
						//self.$Utils.showToast('已收藏', 'none', 1000)
					} else {
						self.$Utils.showToast('', 'none', 1000)
					};
					
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
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
				const postData = {
					type:self.curr
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				
				postData.getAfter = {
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
					signMe: {
						tableName: 'Log',
						searchItem: {
							status:['in',[1,-1]],
							type:7,
							user_no:wx.getStorageSync('user_info').user_no
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
					comment: {
						tableName: 'Message',
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
					share: {
						tableName: 'Log',
						searchItem: {
							status:1,
							type:4,
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:4
						    }
						  ]
						},
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}

					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.getNews(postData, callback);
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
						self.willId = -1;
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
							self.willId = -1;
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
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr;

					self.getMainData(true)
				}
			},
			
			shareBtnShow(index){
				const self = this;
				if(self.mainData[index].id==self.willId){
					self.willId = -1;
					return
				};
				self.willId = self.mainData[index].id;
			},
			
			like(){
				const self = this;
				self.is_like = !self.is_like;
				self.is_shareBtnShow = !self.is_shareBtnShow
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
							title: ['in', ['首页轮播']],
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
			
			realnameshow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_realnameshow = !self.is_realnameshow
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
	.realnameshow{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 42;}
	.realnameshow .bjImg{width: 100%;display: block;}
</style>
