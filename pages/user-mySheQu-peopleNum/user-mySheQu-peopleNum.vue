<template>
	<view>
		
		<view class="whiteBj pdlr4 pdt15 pdb15">
			<view class="ftw">社区人数<text class="fs12 ftn">({{mainData.length}})</text></view>
			<view class="flex SQPerNum pdt10">
				<view class="photo" v-for="(item,index) in mainData" :key="index" :data-user_no ="item.user_no"
				@click="Router.navigateTo({route:{path:'/pages/userHome/userHome?user_no='+$event.currentTarget.dataset.user_no}})">
					<image :src="item.userInfo&&item.userInfo.mainImg&&item.userInfo.mainImg[0]?item.userInfo.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
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
				SQPerNum:[{},{},{},{},{},{},{},{},{},{},{},{},{},{},{}],
				userHomeDate:[{},{}],
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 100
				}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		onLoad(options) {
			const self = this;
			
			self.id = options.id;
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
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 100
					}
				};
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
						self.mainData.push.apply(self.mainData,res.info.data)
						//self.newsData.push.apply(self.newsData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.logGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/userHome.css";
	page{background: #F5F5F5;}
	.SQPerNum{flex-wrap: wrap;}
	.SQPerNum .photo{width: 80rpx;height: 80rpx;margin:0 40rpx 20rpx 0;}
	.SQPerNum .photo image{width: 100%;height: 100%;border-radius: 50%;}
	.SQPerNum .photo:nth-of-type(6n){margin-right: 0;}
</style>
