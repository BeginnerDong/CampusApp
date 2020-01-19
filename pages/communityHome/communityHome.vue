<template>
	<view>
		
		<view class="userHead whiteBj">
			<view class="pdt25 pdlr4 flexRowBetween">
				<view class="flex" style="width:85%;">
					<image class="photo" :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image>
					<view style="width: 70%;">
						<view class="fs16">{{mainData.title}}</view>
					</view>
				</view>
				<view class="gzBtn fs12 center white" style="background: #666;" v-if="mainData.log&&mainData.log.length==0">加入</view>
				<view class="gzBtn fs12 center white" style="background: #666;" v-if="mainData.log&&mainData.log.length>0">已加入</view>
			</view>
		</view>
		<view class="whiteBj pdlr4 fs13 color6">
			<view class="pdtb15"><span class="ftw color2">简介：</span>{{mainData.description}}</view>
		</view>
		
		<view class="fabubtn" v-if="mainData.log&&mainData.log.length>0"
		 @click="Router.navigateTo({route:{path:'/pages/communityFaBu/communityFaBu?id='+mainData.id}})"><image class="icon" src="../../static/images/home-icon1.png" mode=""></image></view>
		
		<view class="mglr4 userHomeLis mgt15">
			<view class="item flexRowBetween" v-for="(item,index) in newsData" :key="index">
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
				userHomeDate:[{},{},{}],
				mainData:{},
				newsData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					thirdapp_id:2,
					status:1,
					id:self.id
				};
				postData.getAfter = {
					log:{
						tableName:'Log',
						middleKey:'id',
						key:'relation_id',
						searchItem:{
							type:6,
							relation_table:'Community',
							user_no:uni.getStorageSync('user_info').user_no
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					self.getNewsData();
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.communityGet(postData, callback);
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
				if(self.mainData.log.length==0){
					postData.searchItem.behavior = 1
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
	.userHead{}
</style>
