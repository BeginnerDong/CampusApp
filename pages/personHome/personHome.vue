<template>
	<view>
		
		<view class="userHead white">
			<view class="headBJ">
				<image :src="submitData.bannerImg.length>0?submitData.bannerImg[0].url:'../../static/images/gerenzhuye-img.png'"  mode=""></image>
			</view>
			<view class="pr" style="z-index: 2;">
				<view class="headTit flexRowBetween pdlr4 pdt15 pdb10 white">
					<view class="item" @click="prev()"><image class="arrowR" style="margin-left: 0;" src="../../static/images/arrowL.png" mode=""></image></view>
					<view class="item fs15 center">个人主页</view>
					<view class="item fs12 flexEnd" 
					@click="upLoadImg('bannerImg')">更换背景图</view>
				</view>
				<view class="pdt30 pdlr4 flex">
					<view @click="upLoadImg('mainImg')" v-if="submitData.mainImg.length>0">
						<image class="photo" :src="submitData.mainImg[0].url" mode=""></image>
					</view>
					<view @click="upLoadImg('mainImg')" v-else>
						<image class="photo" src="../../static/images/about-img.png" mode=""></image>
					</view>
					
					<view style="width: 70%;">
						<view class="fs16">{{userInfoData.name}}</view>
					</view>
				</view>
			</view>
		</view>
		<view class="whiteBj pdlr4 fs13 center" style="height: 100rpx;">
			<view class="radius10 boxShaow flexRowBetween navTwoBox">
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myFollow/user-myFollow'}})">
					<view><image class="icon" src="../../static/images/gerenzhuye-icon.png" mode=""></image></view>
					<view>关注</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/person-follow/person-follow'}})">
					<view><image class="icon" src="../../static/images/gerenzhuye-icon1.png" mode=""></image></view>
					<view>被关注</view>
				</view>
			</view>
		</view>
		<view class="myRowBetween whiteBj">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/person-name/person-name?name='+userInfoData.name}})">
				<view class="ll">昵称</view>
				<view class="rr">
					<text>{{userInfoData.name!=''?userInfoData.name:'输入昵称'}}</text>
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/person-autograph/person-autograph?name='+userInfoData.signature}})">
				<view class="ll">签名</view>
				<view class="rr">
					<text class="color6">{{userInfoData.signature!=''?userInfoData.signature:'输入签名'}}</text>
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/person-role/person-role?name='+userInfoData.role}})">
				<view class="ll">角色</view>
				<view class="rr">
					<text class="color6">{{userInfoData.role!=''?userInfoData.role:'输入角色'}}</text>
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/person-personInfor/person-personInfor'}})">
				<view class="ll">个人资料</view>
				<view class="rr">
					<text class="color6">查看</text>
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="mglr4 userHomeLis mgt15">
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
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				userHomeDate:[{},{}],
				userInfoData:{},
				submitData:{
					mainImg:[],
					bannerImg:[]
				},
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
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
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			prev(){
				const self = this;
				self.$Router.back(1)
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
				postData.user_no = uni.getStorageSync('user_info').user_no;
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
						self.submitData.mainImg = self.userInfoData.mainImg;
						self.submitData.bannerImg = self.userInfoData.bannerImg;
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;			
				uni.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
						self.userInfoUpdate()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getUserToken',
							
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.getUserInfoData()
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/userHome.css";
	@import "../../assets/style/editInfor.css";
	
	page{background: #F5F5F5;}
	.userHead{height: 400rpx;}
	.navTwoBox{background: #fff;border-radius: 10rpx;padding: 30rpx 0 20rpx 0;position: relative;top: -40rpx;}
	.navTwoBox .item{width: 50%;box-sizing: border-box;position: relative;}
	.navTwoBox .item.one:after{content: '';width: 1px;height: 30rpx;background: #F5F5F5;position: absolute;top: 50%;right: 0;transform: translateY(-50%);}
	.navTwoBox .item .icon{width: 36rpx;height: 36rpx;display: block;margin:0 auto 10rpx auto;}
	
	.myRowBetween .item{padding: 30rpx 4%;}
	.myRowBetween .item .rr{font-size: 24rpx;}
</style>
