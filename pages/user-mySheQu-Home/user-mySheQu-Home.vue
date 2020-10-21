<template>
	<view>
		
		<view class="whiteBj pdlr4 pdt15 pdb15">
			<view class="ftw">社区人数<text class="fs12 ftn">({{logData.length}})</text></view>
			<view class="flex SQPerNum pdt10">
				<view class="photo" v-for="(item,index) in logData" :key="index"
						@click="toHome(item.user_no)">
							<image :src="item.userInfo&&item.userInfo.mainImg&&item.userInfo.mainImg[0]?item.userInfo.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
				<view class="photo"  style="overflow: hidden;border-radius: 50%;background-color: #E5E5E5;color: #999;text-align: center;line-height: 80rpx;" @click="Router.navigateTo({route:{path:'/pages/user-mySheQu-peopleNum/user-mySheQu-peopleNum?id='+id}})">
					···
				</view>
			</view>
		</view>
		
		
		<view class="mglr4 userHomeLis mgt15" v-if="newsData.length>0">
			<view class="item flexRowBetween" v-for="(item,index) in newsData" :key="index">
				<view class="data">
					<view class="year">{{Utils.substr(item.menu,0,4)}}</view>
					<view class="day">{{Utils.substr(item.menu,5,10)}}</view>
				</view>
				<view class="infor">
					<view class="list flexRowBetween" v-for="(c_item,c_index) in item.data" 
					@click="toDetail(c_item.type,c_item.id)" :key="c_index">
						<view class="pic" v-if="c_item.mainImg&&c_item.mainImg.length>0">
							<image :src="c_item.mainImg&&c_item.mainImg[0]?c_item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
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
				SQPerNum:[{},{},{},{},{}],
				userHomeDate:[{},{}],
				newsData:[],
				logData:[],
				id:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.id = options.id;
			self.$Utils.loadAll(['getNewsData','getLogData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getNewsData()
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
			
			getLogData() {
				const self = this;
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					relation_id:self.id,
					type:6
					//behavior:['in',[0,1]]
				};
				postData.getAfter = {
					userInfo:{
						tableName:'UserInfo',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['mainImg']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.logData.push.apply(self.logData,res.info.data)
						//self.newsData.push.apply(self.newsData,res.info.data)
					}
					self.$Utils.finishFunc('getLogData');
				};
				self.$apis.logGet(postData, callback);
			},
			
			getNewsData(isNew) {
				const self = this;
				if (isNew) {
					self.newsData = [];
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
					community_id:self.id,
					report:0,
					//behavior:['in',[0,1]]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						if (res.info.data.length > 0) {
							for (var i = 0; i < res.info.data.length; i++) {
								res.info.data[i].create_time = res.info.data[i].create_time.substr(0,10)
								if(self.newsData.length>0){
									var hasone = false;
									for(var j =0;j<self.newsData.length;j++){
										if(res.info.data[i].create_time==self.newsData[j].menu){
											self.newsData[j].data.push(res.info.data[i]);
											hasone = true;
										};
									};
									if(!hasone){
										self.newsData.push({
											menu: res.info.data[i].create_time,
											data:[res.info.data[i]]
										});
									};
								}else{
									self.newsData.push({
										menu: res.info.data[i].create_time,
										data:[res.info.data[i]]
									})
								};
							};
							console.log(self.newsData)
							
							//self.mainData.push.apply(self.mainData,res.info.data)
						}
						//self.newsData.push.apply(self.newsData,res.info.data)
					}
					self.$Utils.finishFunc('getNewsData');
				};
				self.$apis.newsGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/userHome.css";
	page{background: #F5F5F5;}
	.SQPerNum .photo{width: 80rpx;height: 80rpx;margin-right: 40rpx;}
	.SQPerNum .photo image{width: 100%;height: 100%;border-radius: 50%;}
	.SQPerNum .photo:last-child{margin-right: 0;}
</style>
