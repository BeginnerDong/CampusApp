<template>
	<view>
		<view class="orderNav flexRowBetween whiteBj color6 W-Fixed borderB1" style="top: 88rpx;">
			<!-- <view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">动态</view> -->
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">活动</view>
			<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">社区</view>
		</view>
		<view style="height: 82rpx;"></view>
		
		<view class="mglr4">

			<!-- 活动 -->
			<view class="comment  mgt15">
				
				<view class="child" v-for="(item,index) in mainData" :key="index">
					<image v-if="item.signMe.length>0" class="FX-icon" src="../../static/images/activity-icon1.png" mode=""></image>
					<view class="flexRowBetween">
						<view class="flex">
							<view class="photo" :data-user_no ="item.user_no"
						@click="Router.navigateTo({route:{path:'/pages/userHome/userHome?user_no='+$event.currentTarget.dataset.user_no}})">
								<image :src="item.headImg&&item.headImg[0]?item.headImg[0].url:''" mode=""></image>
							</view>
							<view class="name">
								<view class="fs12">{{item.name}}</view>
								<view class="fs10 color6">{{item.create_time}}</view>
							</view>
						</view>
						<view class="pr" v-if="item.type==3">
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
						<view class="lis flex" v-if="item.type==2" @click="clickCollect(index)">
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
						<view class="lis flex" @click="clickGood(index)">
							<image :src="item.goodMe.length>0&&item.goodMe[0].status==1?'../../static/images/home-icon6.png':'../../static/images/home-icon5.png'" mode=""></image>
							<view>{{item.good?item.good.count:0}}</view>
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
				is_show:false,
				curr:1,
				collectData:[{},{}],
				is_shareBtnShow:false,
				is_like:false,
				mainData:[],
				searchItem:{
					type:2,
					report:0,
					user_type:0
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			console.log(2323)
			self.$Utils.loadAll(['getMainData'], self);
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
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr;
					if(self.curr==1){
						self.searchItem.type=2
					}else if(self.curr==2){
						self.searchItem.type=3
					}
					self.getMainData(true)
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
				const postData = {};
				postData.getBefore = {
					test: {
						tableName: 'Log',
						searchItem: {
							relation_table: ['in', ['News']],
							type:['in',[2]],
							user_no:['in',[uni.getStorageSync('user_info').user_no]]
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
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
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
			
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
			
			shareBtnShow(){
				const self = this;
				self.is_shareBtnShow = !self.is_shareBtnShow
			},
			
			like(){
				const self = this;
				self.is_like = !self.is_like;
				self.is_shareBtnShow = !self.is_shareBtnShow
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	page{padding-bottom: 60rpx;}
	
	.zanCont .item{padding: 30rpx 0;}
</style>
