<template>
	<view>
		<view class="orderNav flexRowBetween whiteBj color6 W-Fixed borderB1">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">动态</view>
			<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">活动</view>
			<view class="tt flexCenter" :class="curr==3?'on':''" @click="changeCurr('3')">社区</view>
			<view class="tt flexCenter" :class="curr==4?'on':''" @click="changeCurr('4')">评论</view>
		</view>
		<view style="height: 82rpx;"></view>
		
		<view class="mglr4">
			<!-- 动态 -->
			<view class="comment" v-show="curr==1" v-if="mainData.length>0">
				<view class="child" v-for="(item,index) in mainData" :key="index">
					<view class="fs12" @click="toDetail(item.type,item.id)">{{item.content}}</view>
					<view class="imgbox">
						<view v-for="(c_item,c_index) in item.mainImg" :class="item.mainImg&&item.mainImg.length==1?'lisOne':(item.mainImg&&item.mainImg.length==2?'lisTwo':'lisThree')">
							<image :src="c_item.url" mode="aspectFill" @click="previewImage(index,c_index)"></image>
						</view>
					</view>
					<view class="fs12 color6 pdt10">{{item.create_time}}</view>
					<view class="label pdt10 flexEnd fs13">
						<view class="lis flex">
							<image src="../../static/images/home-icon3.png" mode=""></image>
							<view>{{item.comment?item.comment.count:0}}</view>
						</view>
						<view class="lis flex" @click="clickGood(index)">
							<image :src="item.goodMe&&item.goodMe.length>0&&item.goodMe[0].status==1?'../../static/images/home-icon6.png':'../../static/images/home-icon5.png'" mode=""></image>
							<view>{{item.good?item.good.count:0}}</view>
						</view>
					</view>
				</view>
			</view>	
			
			<!-- 活动 -->
			<view class="comment activeBox" v-show="curr==2"  v-if="mainData.length>0">
				<view class="child pr oh"  v-for="(item,index) in mainData" :key="index">
					<view @click="Router.navigateTo({route:{path:'/pages/myFabu-activityDetail/myFabu-activityDetail'}})">
						<view class="ftw pdb5" @click="toDetail(item.type,item.id)">{{item.title}}</view>
						<view class="fs12 color6" @click="toDetail(item.type,item.id)">{{item.content}}</view>
						<view class="imgbox">
							<view v-for="(c_item,c_index) in item.mainImg" :class="item.mainImg&&item.mainImg.length==1?'lisOne':(item.mainImg&&item.mainImg.length==2?'lisTwo':'lisThree')">
								<image :src="c_item.url" mode="aspectFill" @click="previewImage(index,c_index)"></image>
							</view>
						</view>
					</view>
					<view class="flexRowBetween pdt15 fs12">
						<view class="color6">{{item.create_time}}</view>
						<view class="label flexEnd">
							<view class="lis flex" v-if="item.type!=1" @click="clickCollect(index)">
								<image :src="item.collectMe&&item.collectMe.length>0&&item.collectMe[0].status==1?'../../static/images/activity-icon3.png':'../../static/images/activity-icon2.png'" mode=""></image>
								
								<view>收藏</view>
							</view>
							<view class="lis flex" @click="clickGood(index)">
								<image :src="item.goodMe&&item.goodMe.length>0&&item.goodMe[0].status==1?'../../static/images/home-icon6.png':'../../static/images/home-icon5.png'" mode=""></image>
								<view>{{item.good?item.good.count:0}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 社区 -->
			<view class="comment" v-show="curr==3" v-if="mainData.length>0">
				<view class="child" v-for="(item,index) in mainData" :key="index">
					<view class="fs12" @click="toDetail(item.type,item.id)">{{item.content}}</view>
					<view class="imgbox">
						<view v-for="(c_item,c_index) in item.mainImg" :class="item.mainImg&&item.mainImg.length==1?'lisOne':(item.mainImg&&item.mainImg.length==2?'lisTwo':'lisThree')">
							<image :src="c_item.url" mode="aspectFill" @click="previewImage(index,c_index)"></image>
						</view>
					</view>
					<view class="fs12 color6 pdt10">{{item.create_time}}</view>
					<view class="label pdt15 flexEnd fs13">
						<view class="lis flex">
							<image src="../../static/images/home-icon4.png" mode=""></image>
							<view>{{item.share?item.share.count:0}}</view>
						</view>
						<view class="lis flex">
							<image src="../../static/images/home-icon3.png" mode=""></image>
							<view>{{item.comment?item.comment.count:0}}</view>
						</view>
						<view class="lis flex" @click="clickGood(index)">
							<image :src="item.goodMe&&item.goodMe.length>0&&item.goodMe[0].status==1?'../../static/images/home-icon6.png':'../../static/images/home-icon5.png'" mode=""></image>
							<view>{{item.good?item.good.count:0}}</view>
						</view>
					</view>
				</view>
			</view>	
			
			<!-- 评论 -->
			<view class="zanCont" v-show="curr==4" v-if="mainData.length>0">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view class="pdb10 fs13">{{item.content}}</view>
					<view class="cont f5bj flex">
						<view class="pic" v-if="item.news&&item.news[0]&&item.news[0].mainImg
						&&item.news[0].mainImg.length>0">
						<image :src="item.news&&item.news[0]&&item.news[0].mainImg&&
						item.news[0].mainImg[0]?item.news[0].mainImg[0].url:''" mode=""></image>
						</view>
						<view class="rr">
							<view class="tit fs13 avoidOverflow">{{item.news&&item.news[0]?item.news[0].title:''}}</view>
							<view class="text avoidOverflow2 fs12 color6">{{item.news&&item.news[0]?item.news[0].content:''}}</view>
						</view>
					</view>
				</view>
			</view>
			
			<view v-else>
				<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
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
				fabuData:[{},{}],
				searchItem:{
					thirdapp_id:2,
					report:0,
					type:1
				},
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr;
					if(self.curr!=4){
						self.searchItem.type=self.curr;
						self.getMainData(true)
					}else if(self.curr==4){
						
						self.getMessageData(true)
					}
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
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
			
			getMessageData(isNew) {
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
					type:2,
					status:1,
					passage1:['in',['']],
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getAfter = {
					news: {
						tableName: 'News',
						searchItem: {
							status:['in',[1,-1]],
						},
						middleKey: 'relation_id',
						key: 'id',
						condition: 'in',
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
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/comment.css";
	@import "../../assets/style/zanCont.css";
	page{padding-bottom: 60rpx;}
	
	.zanCont .item{padding: 30rpx 0;}
</style>
