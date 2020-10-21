<template>
	<view>
		
		<view class="comment mglr4">
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo" @click="toHome(originData.user_no)">
							<image :src="originData.headImg&&originData.headImg[0]?originData.headImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view class="name">
							<view class="fs12">{{originData.name!=''?originData.name:'用户'+originData.user_no}}</view>
							<view class="fs10 color6">{{originData.create_time}}</view>
						</view>
					</view>
					<view class="flexEnd" v-if="me==originData.user_no" @click="deleteThis()">
						<view class="gzBtn fs12 center white color6Bj">删除</view>
					</view>
					<view class="pr" v-if="originData.type==3">
						<view class="flexEnd" @click="shareBtnShow">
							<view class="dian"></view>
							<view class="dian"></view>
							<view class="dian"></view>
						</view>
						<view class="fx-shareBtn fs11 white" v-show="is_shareBtnShow">
							<view class="flexCenter pdb10" @click="like">
								<image class="icon" :src="is_like==true?'../../static/images/home-icon9.png':'../../static/images/home-icon7.png'" mode=""></image>
								<text>收藏</text>
							</view>
							<view class="flexCenter" @click="Router.navigateTo({route:{path:'/pages/report/report'}})">
								<image class="icon" src="../../static/images/home-icon8.png" mode=""></image>
								<text>举报</text>
							</view>
						</view>
					</view>
					
				</view>
				<view class="font14 pdt10">{{originData.content}}</view>
				<view class="imgbox">
					
					<view class="img" v-for="(item,index) in originData.mainImg" :key="index" :class="originData.mainImg.length==1?'lisOne':(originData.mainImg.length==2?'lisTwo':'lisThree')">
						<image :src="item.url" mode="aspectFill" @click="previewImage(index)"></image>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="pdlr4">
			<view class="pdt15 ftw">评论</view>
			<view class="pinglunLis" v-if="mainData.length>0">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view class="flex">
						<view class="pler-photo" @click="toHome(item.user_no)">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view class="name mgl10">
							<view class="fs12 color6">{{item.title!=''?item.title:'用户'+item.user_no}}</view>
							<view class="fs10 color9">{{item.create_time}}</view>
						</view>
					</view>
					<view class="pdtb10 fs12">{{item.content}}</view>
					<view class="flexRowBetween">
						<view class="flex fs11 numHF"  :data-id="item.id"
							@click="Router.navigateTo({route:{path:'/pages/post-plDetail/post-plDetail?id='+$event.currentTarget.dataset.id}})">
							{{item.comment?item.comment.count:0}}条回复&gt;
						</view>
						<view class="flexEnd fs12" @click="reply(index)">
							<image class="plIcon mgr10" src="../../static/images/details-icon.png" mode=""></image>
							<text>回复</text>
						</view>
					</view>
				</view>
			</view>
			<view v-else>
				<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
			</view>
		</view>
		<view class="fx-PlZan pdlr4 fs12 color6 borderB1" style="bottom: 80rpx;display: flex;" v-if="submitDataTwo.passage1==''&&isShowInput">
			<view class="flexRowBetween" style="width: 100%;">
				<view class="input" style="width: 85%;">
					<input v-model="submitData.content" placeholder="请输入评论..." />
				</view>
				<view class="rrBtn">
					<button @click="addMessage()">发送</button>
				</view>
			</view>
		</view>
		<view class="fx-PlZan pdlr4 fs12 color6 borderB1" style="bottom: 80rpx;display: flex;" v-if="submitDataTwo.passage1!=''">
			<view class="flexRowBetween" style="width: 100%;">
				<view class="input" style="width: 85%;">
					<input v-model="submitDataTwo.content" placeholder="请输入回复..."/>
				</view>
				<view class="rrBtn">
					<button  @click="addMessageTwo()">发送</button>
				</view>
			</view>
		</view>
		<view class="fx-PlZan pdlr4 flexRowBetween fs12 color6">
			<view class="tt flexCenter" @click="copy()" v-if="originData.type==3">
				<image class="icon" src="../../static/images/home-icon4.png" mode=""></image>
				<text>分享</text>
			</view>
			<view class="tt flexCenter" @click="showInput">
				<image class="icon" src="../../static/images/home-icon3.png" mode=""></image>
				<text>评论</text>
			</view>
			<view class="tt flexCenter" @click="clickGood">
				<image class="icon" :src="originData.goodMe&&originData.goodMe.length>0&&originData.goodMe[0].status==1?'../../static/images/home-icon6.png':'../../static/images/home-icon5.png'" mode=""></image>
				<text>赞</text>
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
				pinglunData:[{},{},{}],
				is_shareBtnShow:false,
				is_like:false,
				originData:{},
				mainData:[],
				submitData:{
					content:'',
					type:2,
					title:'',
					mainImg:[],
					relation_table:'News',
					relation_user:'',
					relation_id:''
				},
				submitDataTwo:{
					content:'',
					type:2,
					title:'',
					mainImg:[],
					relation_table:'News',
					relation_user:'',
					relation_id:'',
					passage1:''
				},
				messageId:'',
				isShowInput:'',
				me:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.submitData.title=uni.getStorageSync('user_info').info.name;
			self.submitData.mainImg=uni.getStorageSync('user_info').info.mainImg;
			self.submitDataTwo.title=uni.getStorageSync('user_info').info.name;
			self.submitDataTwo.mainImg=uni.getStorageSync('user_info').info.mainImg;
			self.me = uni.getStorageSync('user_info').user_no;
			self.$Utils.loadAll(['getOriginData','getMainData','getShareData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
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
			
			copy(){
				const self = this;
				uni.setClipboardData({
				    data: self.shareData.url,
				    success: function () {
				        self.$Utils.showToast('分享链接已复制', 'none')
				    }
				});
			},
			
			getShareData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					title:'分享'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.shareData = res.info.data[0];
					}
					self.$Utils.finishFunc('getShareData');
				};
				self.$apis.labelGet(postData, callback);
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
									id: self.originData.id
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
			
			previewImage: function(index) {
				const self = this;
				var imageList = [];
				var current = self.originData.mainImg[index].url;
				for (var i = 0; i < self.originData.mainImg.length; i++) {
					imageList.push(self.originData.mainImg[i].url)
				}
				uni.previewImage({
					current: current,
					urls: imageList
				})
			},
			
			showInput(){
				const self = this;
				self.submitDataTwo.passage1 = '';
				self.isShowInput = !self.isShowInput
			},
			
			reply(index){
				const self =  this;
				console.log(index)
				//self.messageId = self.mainData[index].id;
				self.submitDataTwo.passage1 = self.mainData[index].id;
				self.submitDataTwo.relation_user = self.mainData[index].user_no;
				console.log(self.submitDataTwo.passage1)
			},
			
			shareBtnShow(){
				const self = this;
				self.is_shareBtnShow = !self.is_shareBtnShow
			},
			
			like(){
				const self = this;
				self.is_like = !self.is_like;
				self.is_shareBtnShow = !self.is_shareBtnShow
			},
			
			getOriginData() {
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
							user_no:wx.getStorageSync('user_info').user_no,
							
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.originData = res.info.data[0];
						self.submitData.relation_id = self.originData.id;
						self.submitData.relation_user = self.originData.user_no;
						self.submitDataTwo.relation_id = self.originData.id;
					}
					self.$Utils.finishFunc('getOriginData');
				};
				self.$apis.newsGet(postData, callback);
			},
			
			clickGood() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (self.originData.goodMe.length == 0) {
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
					relation_id: self.originData.id,
					relation_table:'News',
					user_no: uni.getStorageSync('user_info').user_no,
					relation_user:self.originData.user_no
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.originData.goodMe.push({
							status: 1,
							id: res.info.id
						});
						
						//self.$Utils.showToast('已收藏', 'none', 1000)
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateGoodLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.originData.goodMe[0].id
						
					},
					data: {
						status: -self.originData.goodMe[0].status
					}
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.originData.goodMe[0].status = -self.originData.goodMe[0].status;
						
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
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
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_type:0,
					type:2,
					status:1,
					passage1:['in',['']],
					relation_id:self.id
				};
				postData.getAfter = {
					comment: {
						tableName: 'Message',
						searchItem: {
							status:1,
							type:2,
							user_type:0
						},
						middleKey: 'id',
						key: 'passage1',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:2,user_type:0
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
				self.$apis.messageGet(postData, callback);
			},
			
			addMessage(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.content==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('评论不能为空', 'none', 1000);
					return
				};
				const postData = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.submitData.content = '';
						self.getMainData(true)
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.messageAdd(postData, callback);
			},
			
			addMessageTwo() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitDataTwo.content==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('评论不能为空', 'none', 1000);
					return
				};
				const postData = {};
				postData.data = self.$Utils.cloneForm(self.submitDataTwo);
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.submitDataTwo.content = '';
						self.getMainData(true)
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.messageAdd(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	page{padding-bottom: 100rpx;}
	.comment .child{border-bottom: 0;}
	.fx-shareBtn{width: 200rpx;padding: 30rpx 0rpx;border-radius: 10rpx;background: rgba(0,0,0,0.5);position: absolute;top: 30rpx;right: 10rpx;z-index: 2;}
	.fx-shareBtn .icon{width: 28rpx;height: 28rpx;margin-right: 10rpx; }
	
	.fx-PlZan .input{width: 85%;}
	.fx-PlZan .input input{width: 100%;background: #fff;line-height: 50rpx;height: 50rpx;border-radius: 8rpx; display: block;box-sizing: border-box;padding: 0 20rpx;font-size: 26rpx;}
	.fx-PlZan .rrBtn{width: 12%;}
	.fx-PlZan .rrBtn button{font-size: 26rpx;width: 100%;height: 50rpx;line-height: 50rpx;text-align: center;background: #222; color: #fff;}
</style>
