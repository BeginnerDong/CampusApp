<template>
	<view>
		<view   style="padding-top: 44px;" class="W-Fixed">
			<view class="flexRowBetween indexTit borderB1">
				<view class="userPhoto" @click="Router.navigateTo({route:{path:'/pages/user/user'}})">
					<image :src="userInfoData.mainImg&&userInfoData.mainImg.length>0?userInfoData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
				</view>
				<view class="fs16 color6">发现</view>
				<view @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})"><image class="seachBtn" src="../../static/images/home-icon.png" mode=""></image></view>
			</view>
			
			<view class="orderNav flexRowBetween whiteBj color6   borderB1">
				<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">社区</view>
				<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">已加入</view>
				<view class="tt flexCenter" :class="curr==3?'on':''" @click="changeCurr('3')">更多</view>
			</view>
		</view>
		<view style="height: 83px;background-color: #f5f5f5;"></view>
		<view class="pdtb25"></view>
		<view class="f5H5"></view>
		
		<view class="comment mglr4" v-show="curr==1">
			<view v-if="mainData.length>0">
				<view class="child" v-for="(item,index) in mainData" :key="index">
					<view class="flexRowBetween">
						<view class="flex">
							<view class="photo" @click="toHome(item.user_no)">
								<image :src="item.headImg&&item.headImg[0]?item.headImg[0].url:'../../static/images/about-img.png'" mode=""></image>
							</view>
							<view class="name">
								<view class="fs12">{{item.name!=''?item.name:'用户'+item.user_no}}</view>
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
					<view class="">
						
						<view class="fs12 pdt10" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two?id='+$event.currentTarget.dataset.id}})">{{item.content}}</view>
						<view class="imgbox" >
							<view class="img" v-for="(c_item,c_index) in item.mainImg" :class="item.mainImg.length==1?'lisOne':(item.mainImg.length==2?'lisTwo':'lisThree')">
								<image :src="c_item.url" mode="aspectFill" @click="previewImage(index,c_index)"></image>
							</view>
						</view>
					</view>
					<view class="label pdt15 flexEnd fs13">
						<view class="lis flex" v-if="item.type==3" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two?id='+$event.currentTarget.dataset.id}})">
							<image src="../../static/images/home-icon4.png" mode=""></image>
							<view>{{item.share?item.share.count:0}}</view>
						</view>
						<view class="lis flex" v-if="item.type!=2" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two?id='+$event.currentTarget.dataset.id}})">
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
		</view>
		
		<view class="myRowBetween pdlr4" v-show="curr==2">
			<view v-if="hasAddData.length>0">
				<view class="item flexRowBetween" v-for="(item,index) in hasAddData" :key="index" >
					<view class="ll flex" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/communityHome/communityHome?id='+$event.currentTarget.dataset.id}})">
						<view class="photo">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view class="ll-tit">
							<view class="fs14">{{item.title}}</view>
							<view class="fs12 color9 mgt5 avoidOverflow">{{item.description}}</view>
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
			<view v-else>
				<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
			</view>
		</view>
		
		<view class="myRowBetween pdlr4" v-show="curr==3">
			<view v-if="noAddData.length>0" >
				<view class="item flexRowBetween" v-for="(item,index) in noAddData" :key="index">
					<view class="ll flex" :data-id="item.id" 
					@click="Router.navigateTo({route:{path:'/pages/communityHome/communityHome?id='+$event.currentTarget.dataset.id}})">
						<view class="photo">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view class="ll-tit">
							<view class="fs14">{{item.title}}</view>
							<view class="fs12 color9 mgt5 avoidOverflow">{{item.description}}</view>
						</view>
					</view>
					<view class="rr" v-if="userInfoData.check_status==2">
						<view class="joinOK color9 fs12"  @click="joinCommunity(index)">
							<text>加入</text>
						</view>
					</view>
					<view class="rr" v-if="userInfoData.check_status!=2" @click="realnameshow">
						<view class="joinOK color9 fs12">
							<text>加入</text>
						</view>
					</view>
				</view>
			</view>
			<view v-else>
				<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
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
			<view class="navbar_item">
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
				mainData:[],
				hasAddData:[],
				noAddData:[],
				userInfoData:{},
				willId:-1,
				is_realnameshow:false
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
			if(self.curr==1){
				self.mainData = [];
				self.$Utils.loadAll(['getMainData'], self);
				
			}else if(self.curr==2){
				self.hasAddData = [];
				self.$Utils.loadAll(['getHasAddData'], self);
				
			}else if(self.curr==3){
				self.noAddData = [];
				self.$Utils.loadAll(['getNoAddData'], self);
				
			}
		},
		
		methods: {
			
			
			toHome(user_no){
				const self = this;
				if(user_no==uni.getStorageSync('user_info').user_no){
					self.Router.navigateTo({route:{path:'/pages/personHome/personHome'}})
				}else{
					self.Router.navigateTo({route:{path:'/pages/userHome/userHome?user_no='+user_no}})
				}
			},
			
			realnameshow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_realnameshow = !self.is_realnameshow
			},
			
			previewImage: function(index,c_index) {
				const self = this;
				var imageList = [];
				var current = self.mainData[index].mainImg[c_index].url;
				for (var i = 0; i < self.mainData[index].mainImg.length; i++) {
					imageList.push(self.mainData[index].mainImg[i].url)
				}
				uni.previewImage({
					current: current,
					urls: imageList
				})
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
						self.mainData = [];
						self.$Utils.loadAll(['getMainData'], self);
					
					}else if(self.curr==2){
						self.hasAddData = [];
						self.$Utils.loadAll(['getHasAddData'], self);
						
					}else if(self.curr==3){
						self.noAddData = [];
						self.$Utils.loadAll(['getNoAddData'], self);
						
					}
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
			alertBoxShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_alertBox = !self.is_alertBox
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
				const postData = {
					type:3
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
					uni.stopPullDownRefresh();
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.getNews(postData, callback);
			},
			
			getHasAddData(isNew) {
				const self = this;
				if (isNew) {
					self.hasAddData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.getBefore = {
					test: {
						tableName: 'Log',
						searchItem: {
							relation_table: ['in', ['Community']],
							type:['in',[6]],
							user_no:['in',[uni.getStorageSync('user_info').user_no]]
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					thirdapp_id:2,
					status:1
				};
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.hasAddData.push.apply(self.hasAddData,res.info.data);
					}
					uni.stopPullDownRefresh();
					self.$Utils.finishFunc('getHasAddData');
				};
				self.$apis.communityGet(postData, callback);
			},
			
			getNoAddData(isNew) {
				const self = this;
				if (isNew) {
					self.noAddData = [];
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
						self.noAddData.push.apply(self.noAddData,res.info.data);
					}
					uni.stopPullDownRefresh();
					self.$Utils.finishFunc('getNoAddData');
				};
				self.$apis.getUnfollow(postData, callback);
			},
			
			joinCommunity(index) {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确定加入该社区？',
					showCancel:true,
					success: function(res) {
						if (res.confirm) {
							const postData = {};
							postData.tokenFuncName = 'getUserToken';
							
							postData.data = {};
							postData.data = {
								type:6,
								relation_table:'Community',
								relation_id:self.noAddData[index].id,
							};
							const callback = (data) => {				
								if (data.solely_code == 100000) {					
									self.$Utils.showToast('已加入', 'none', 1000)
									setTimeout(function() {
										self.getNoAddData(true)
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast(data.msg, 'none', 1000)
								}	
							};
							self.$apis.logAdd(postData, callback);
						} else if (res.cancel) {
							uni.setStorageSync('canClick', true);	
							console.log('用户点击取消');
						}
					}
				});
				
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
				self.willId = -1;
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
	.realnameshow{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 42;}
	.realnameshow .bjImg{width: 100%;display: block;}
</style>
