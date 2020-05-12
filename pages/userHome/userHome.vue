<template>
	<view>
		
		<view class="userHead white pr">
			<view class="headBJ">
				<image :src="userInfoData.bannerImg&&userInfoData.bannerImg.length>0?userInfoData.bannerImg[0].url:'../../static/images/gerenzhuye-img.png'"  mode=""></image>
			</view>
			<view class="pr" style="z-index: 2;">
				<view class="flexRowBetween pdlr4 pdt25 pdb10 white">
					<view @click="prev()"><image class="arrowR" style="margin-left: 0;" src="../../static/images/arrowL.png" mode=""></image></view>
					<view class="fs15">ta的主页</view>
					<view class="gzBtn fs12 center color6 whiteBj" @click="Utils.stopMultiClick(getChatID)">私信</view>
				</view>
				<view class="pdt30 pdlr4 flexRowBetween">
					<view class="flex" style="width:85%;">
						<image class="photo" :src="userInfoData.mainImg&&userInfoData.mainImg.length>0?userInfoData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						<view style="width: 70%;">
							<view class="fs16">{{userInfoData.name}}</view>
						</view>
					</view>
					<view class="gzBtn fs12 center color6 whiteBj" style="width: 140rpx;"  @click="Utils.stopMultiClick(submit)" v-if="userInfoData.log&&userInfoData.log.length>0&&userInfoData.log[0].status==1">取消关注</view>
					<view class="gzBtn fs12 center color6 whiteBj"  @click="Utils.stopMultiClick(submit)" v-if="userInfoData.log&&userInfoData.log.length==0">关注</view>
				</view>
			</view>
		</view>
		<view class="whiteBj pdlr4 fs13 color6">
			<view class="pdtb15"><span class="ftw color2">签名：</span>{{userInfoData.signature}}</view>
		</view>
		
		<view class="mglr4 userHomeLis mgt15" v-if="mainData.length>0">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="data">
					<view class="year">{{Utils.substr(item.menu,0,4)}}</view>
					<view class="day">{{Utils.substr(item.menu,5,10)}}</view>
				</view>
				<view class="infor">
					<view class="list flexRowBetween" v-for="(c_item,c_index) in item.data" 
					@click="toDetail(c_item.type,c_item.id)">
						<view class="pic" v-if="c_item.mainImg&&c_item.mainImg.length>0">
							<image :src="c_item.mainImg&&c_item.mainImg[0]?c_item.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="rr">
							<view class="pdt5 pdb5 avoidOverflow">{{c_item.title}}</view>
							<view class="fs12 color6 avoidOverflow2">{{c_item.content}}</view>
						</view>
					</view>
					
				</view>
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
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				userHomeDate:[{},{}],
				userInfoData:{},
				mainData:[],
				user_no:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.user_no = options.user_no;
			self.$Utils.loadAll(['getUserInfoData','getMainData'], self);
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
			
			prev(){
				const self = this;
				self.$Router.back(1)
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
			
			getChatID() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.user_no = self.user_no;
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.Router.navigateTo({route:{path:'/pages/userHome-tidings/userHome-tidings?id='+res.info.id}})
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.getChatID(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: self.user_no
				};
				postData.getAfter = {
					log:{
						tableName:'Log',
						middleKey:'user_no',
						key:'relation_user',
						searchItem:{
							type:5,
							status:['in',[-1,1]],
							user_no:uni.getStorageSync('user_info').user_no
						},
						condition:'='
					}
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
						pagesize: 10
					}
				};
				const postData = {
					type:4
				};
				postData.user_no = self.user_no;
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						for (var i = 0; i < res.info.data.length; i++) {
							res.info.data[i].create_time = res.info.data[i].create_time.substr(0,10)
							if(self.mainData.length>0){
								var hasone = false;
								for(var j =0;j<self.mainData.length;j++){
									if(res.info.data[i].create_time==self.mainData[j].menu){
										self.mainData[j].data.push(res.info.data[i]);
										hasone = true;
									};
								};
								if(!hasone){
									self.mainData.push({
										menu: res.info.data[i].create_time,
										data:[res.info.data[i]]
									});
								};
							}else{
								self.mainData.push({
									menu: res.info.data[i].create_time,
									data:[res.info.data[i]]
								})
							};
						};
						console.log(self.mainData)
						
						//self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.getNews(postData, callback);
			},
			
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (self.userInfoData.log.length == 0) {
					self.addLog()
				} else {
					self.updateLog()
				};
			},
			
			addLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 5,
					title: '关注成功',
					relation_user: self.user_no,
					relation_table:'User',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.$Utils.showToast('关注成功', 'none', 1000)
						setTimeout(function() {
							self.getUserInfoData()
						}, 1000);
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.userInfoData.log[0].id
					},
					data: {
						status: -self.userInfoData.log[0].status
					}
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.userInfoData.log[0].status = -self.userInfoData.log[0].status;
						if(self.userInfoData.log[0].status==1){
							self.$Utils.showToast('关注成功', 'none', 1000)
						}else{
							self.$Utils.showToast('取消成功', 'none', 1000)
						}
						setTimeout(function() {
							self.getUserInfoData()
						}, 1000);
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
	@import "../../assets/style/userHome.css";
	page{background: #F5F5F5;}
	.userHead{height: 400rpx;}
	
</style>
