<template>
	<view>
		
		<view class="comment mglr4 activeBox">
			<view class="child">
				<image v-if="mainData.signMe&&mainData.signMe.length>0" class="FX-icon" src="../../static/images/activity-icon1.png" mode=""></image>
				<view class="flexRowBetween">
					
					<view class="flex">
						<view class="photo">
							<image :src="mainData.headImg&&mainData.headImg[0]?mainData.headImg[0].url:''" mode=""></image>
						</view>
						<view class="name">
							<view class="fs12">{{mainData.name}}</view>
							<view class="fs10 color6">{{mainData.create_time}}</view>
						</view>
					</view>
					<view class="flexEnd" v-if="me==mainData.user_no" @click="deleteThis()">
						<view class="gzBtn fs12 center white color6Bj">删除</view>
					</view>
				</view>
				<view class="ftw pdt10 pdb5">{{mainData.title}}</view>
				<view class="fs12 color6">{{mainData.content}}</view>
				<view class="imgbox">
					
					<view class="img" v-for="(item,index) in mainData.mainImg" :class="mainData.mainImg.length==1?'lisOne':(mainData.mainImg.length==2?'lisTwo':'lisThree')">
						<image :src="item.url" mode="aspectFill" @click="previewImage(index)"></image>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
					<view class="lis flex"  @click="clickCollect()">
						<image :src="mainData.collectMe&&mainData.collectMe.length>0&&mainData.collectMe[0].status==1?'../../static/images/activity-icon3.png':'../../static/images/activity-icon2.png'" mode=""></image>
						<view>收藏</view>
					</view>
					<view class="lis flex" @click="clickGood()">
						<image :src="mainData.goodMe&&mainData.goodMe.length>0&&mainData.goodMe[0].status==1?'../../static/images/home-icon6.png':'../../static/images/home-icon5.png'" mode=""></image>
						<view>{{mainData.good?mainData.good.count:0}}</view>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="pdlr4 pdb10">
			<view class="ftw pdt15 pdb10">参与条件</view>
			<view class="fs12 color6 xqInfor">
				{{mainData.condition}}
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="pdlr4">
			<view class="flexRowBetween pdtb15 borderB1">
				<view class="fs14">开始时间</view>
				<view class="fs13">{{mainData.start_time}}</view>
			</view>
			<view class="flexRowBetween pdtb15">
				<view class="fs14">活动地址</view>
				<view class="fs13">{{mainData.address}}</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="pdtb25 mglr4">
			<view class="submitbtn" v-if="mainData.signMe&&mainData.signMe.length==0">
				<button class="btn" type="button"  @click="$Utils.stopMultiClick(signUp)">报名参加</button>
			</view>
			<view class="center fs13 color6 pdtb15" v-if="mainData.signMe&&mainData.signMe.length>0">报名成功，可以在我的-参加活动中查看入场券</view>
		</view>
		
		
		<!-- 报名提示弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<!-- <view class="alertBox center pdt20 radius10 whiteBj" v-show="is_alertBox">
			<view>提示：</view>
			<view class="pdt10 pdb20 color6 fs13">确定参加此次活动？</view>
			<view class="flexRowBetween selt seltBtn">
				<view class="half" @click="alertBoxShow">取消</view>
				<view class="half" @click="signUpOKShow">确定</view>
			</view>
		</view> -->
		
		<view class="signUpOK radius10 whiteBj" v-show="is_signUpOK">
			<view class="closeBtn" @click="Router.redirectTo({route:{path:'/pages/activity/activity'}})">×</view>
			<view class="center pdb20">报名成功！</view>
			<view class="quanCard fs12">
				<view class="center fs16 quanTit pdb10">入场券</view>
				<view class="">时间：{{mainData.start_time}}</view>
				<view>地点：{{mainData.address}}</view>
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
				is_signUpOK:false,
				mainData:{},
				me:''
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
			self.me = uni.getStorageSync('user_info').user_no
		},
		
		methods: {
			
			previewImage(index) {
				const self = this;
				var imageList = [];
				var current = self.mainData.mainImg[index].url;
				for (var i = 0; i < self.mainData.mainImg.length; i++) {
					imageList.push(self.mainData.mainImg[i].url)
				}
				uni.previewImage({
					current: current,
					urls: imageList
				})
			},
			
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
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					thirdapp_id:2,
					report:0,
					user_type:0,
					id:self.id
				};
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
					
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.mainData.start_time = self.$Utils.timeto(self.mainData.start_time*1000,'ymd')
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.newsGet(postData, callback);
			},
			
			
			
			clickGood() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (self.mainData.goodMe.length == 0) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},
			
			addGoodLog(index) {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '点赞成功',
					relation_id: self.mainData.id,
					relation_table:'News',
					user_no: uni.getStorageSync('user_info').user_no,
					relation_user:self.mainData.user_no
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData.goodMe.push({
							status: 1,
							id: res.info.id
						});
						self.mainData.good.count = self.mainData.good.count+1
						//self.$Utils.showToast('已收藏', 'none', 1000)
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			signUp() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				uni.showModal({
					title: '提示',
					content: '确定参加此次活动？',
					showCancel:true,
					success: function(res) {
						if (res.confirm) {
							const postData = {};
							postData.data = {
								type: 7,
								title: '活动报名',
								relation_id: self.mainData.id,
								relation_table:'News',
								user_no: uni.getStorageSync('user_info').user_no,
								relation_user:self.mainData.user_no
							};
							postData.tokenFuncName = 'getUserToken';
							const callback = (res) => {
								if (res.solely_code == 100000) {
									self.is_show = !self.is_show;
									self.is_signUpOK = !self.is_signUpOK
									//self.$Utils.showToast('已收藏', 'none', 1000)
								} else {
									self.$Utils.showToast(res.msg, 'none', 1000)
								};
								uni.setStorageSync('canClick', true);	
							};
							self.$apis.logAdd(postData, callback);
						} else if (res.cancel) {
							uni.setStorageSync('canClick', true);	
							console.log('用户点击取消');
						}
					}
				});
				
			},
			
			
			updateGoodLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.mainData.goodMe[0].id
						
					},
					data: {
						status: -self.mainData.goodMe[0].status
					}
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData.goodMe[0].status = -self.mainData.goodMe[0].status;
						if(self.mainData.goodMe[0].status==1){
							self.mainData.good.count = self.mainData.good.count+1
							//self.$Utils.showToast('已收藏', 'none', 1000)
						}else{
							self.mainData.good.count = self.mainData.good.count-1
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
				if (self.mainData.collectMe.length == 0) {
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
					relation_id: self.mainData.id,
					relation_table:'News',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData.collectMe.push({
							status: 1,
							id: res.info.id
						});
						self.mainData.collect.count = self.mainData.collect.count+1
						//self.$Utils.showToast('已收藏', 'none', 1000)
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			
			deleteThis() {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确定删除此条动态/活动吗？',
					showCancel:true,
					success: function(res) {
						if (res.confirm) {
							const postData = {
								searchItem: {
									id: self.mainData.id
								},
								data: {
									status: -1
								}
							};
							postData.tokenFuncName = 'getUserToken';
							const callback = (res) => {
								uni.setStorageSync('canClick', true);
								if (res.solely_code == 100000) {
									self.$Utils.showToast('删除成功', 'none', 1000)
									setTimeout(function() {
										uni.navigateBack({
											delta:1
										})
									}, 1000);
								} else {
									self.$Utils.showToast(res.msg, 'none', 1000)
								};
							};
							self.$apis.newsUpdate(postData, callback);
						} else if (res.cancel) {
							uni.setStorageSync('canClick', true);	
							console.log('用户点击取消');
						}
					}
				});
			},
			
			
			updateCollectLog(index) {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.mainData.collectMe[0].id
						
					},
					data: {
						status: -self.mainData.collectMe[0].status
					}
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData.collectMe[0].status = -self.mainData.collectMe[0].status;
						if(self.mainData.collectMe[0].status==1){
							self.mainData.collect.count = self.mainData.collect.count+1
							//self.$Utils.showToast('已收藏', 'none', 1000)
						}else{
							self.mainData.collect.count = self.mainData.collect.count-1
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
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	@import "../../assets/style/alertBox.css";
	.comment .child{border-bottom: 0;}
	.xqInfor view{margin-bottom: 10rpx;}
	
	/* 报名成功弹框 */
	.signUpOK{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 60;box-sizing: border-box;padding: 50rpx 30rpx 120rpx 30rpx;}
	
	
	.quanCard{width: 100%;height: 220rpx;padding: 30rpx 30rpx 30rpx 190rpx;background: url(../../static/images/canyuhuodong-img.png) no-repeat 0 0/100% 100%;box-sizing: border-box;}
</style>
