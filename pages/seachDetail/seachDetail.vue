<template>
	<view>
		
		<view class="comment mglr4 mgt15" v-if="mainData.length>0">
			<view class="" style="border-bottom: 1px solid #eee;padding: 15px 0;"  v-if="item.on_shelf&&item.on_shelf==1" v-for="(item,index) in mainData"   :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/integralShop-detail/integralShop-detail?id='+$event.currentTarget.dataset.id}})">
				<view class="item radius10 whiteBj">
					<view class="fs15 ftw">{{item.title}}</view>
					<view class="fs12 color6 pdtb5">{{item.shop?item.shop.address:''}}</view>
					<view class="price fs16 ftw">{{item.price}}</view>
					<view class="ThreeImg flex">
						<view class="imgs" v-for="c_item in item.mainImg"><image :src="c_item.url" mode=""></image></view>
					</view>
				</view>
			</view>
			<view class="child" v-if="!item.on_shelf" v-for="(item,index) in mainData">
				<image v-if="item.signMe.length>0" class="FX-icon" src="../../static/images/activity-icon1.png" mode=""></image>
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
					<view class="ftw pdt10 pdb5" v-if="item.type==2"  @click="toDetail(item.type,item.id)">{{item.title}}</view>
					<view class="fs12" :class="item.type!=2?'pdt10':''"  @click="toDetail(item.type,item.id)">{{item.content}}</view>
					<view class="imgbox" style="padding-top: 20rpx;">
						<view class="img" v-for="(c_item,c_index) in item.mainImg" :class="item.mainImg.length==1?'lisOne':(item.mainImg.length==2?'lisTwo':'lisThree')">
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
					<view class="lis flex" v-if="item.type==3"  @click="toDetail(item.type,item.id)">
						<image src="../../static/images/home-icon4.png" mode=""></image>
						<view>{{item.share?item.share.count:0}}</view>
					</view>
					<view class="lis flex" v-if="item.type!=2"  @click="toDetail(item.type,item.id)">
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
		<view v-else>
			<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
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
		
		
		
		onLoad(options) {
			const self = this;
			if(options.keywords){
				self.keywords = options.keywords;
			}
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData','getMainData'], self);
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
			
			realnameshow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_realnameshow = !self.is_realnameshow
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
				if(self.keywords){
					postData.keywords = self.keywords
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
				self.$apis.searchAll(postData, callback);
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
		}
	};
</script>

<style>
	
	@import "../../assets/style/head.css";
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	@import "../../assets/style/food.css";
	page{padding-bottom: 60rpx;}
	.realnameshow{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 42;}
	.realnameshow .bjImg{width: 100%;display: block;}
</style>

