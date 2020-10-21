<template>
	<view>
		<view class="mglr4 radius10 whiteBj mgt15 canTing">
			<view class="flex">
				<view class="pic">
					<image :src="mainData.shop&&mainData.shop[0]&&mainData.shop[0].mainImg
					&&mainData.shop[0].mainImg[0]?mainData.shop[0].mainImg[0].url:''"
					 mode=""></image>
				</view>
				<view class="infor mgl10  fs12 color6" style="width: 70%;">
					<view class="fs14 color2 ftw">{{mainData.shop&&mainData.shop[0]?mainData.shop[0].name:''}}</view>
					<!-- <view class="flex" style="flex-wrap: wrap;">
						<view class="lable">甜点</view>
					</view> -->
					<view class="adrs mgt15">{{mainData.shop&&mainData.shop[0]?mainData.shop[0].address:''}}</view>
				</view>
			</view>
		</view>

		<view class="mglr4 whiteBj radius8 mgt15 pdlr4">
			<view class="pdtb15 flex borderB1">
				<view class="mgr10 fs13">总体</view>
				<view class="starBox mgr5">
					<image v-for="item in stars" @click="select(item)" :src="item<=submitData.score?'../../static/images/the-store-icon4.png':'../../static/images/the-store-icon5.png'"
					 mode=""></image>
				</view>
			</view>
			<view class="borderB1 mgb15">
				<textarea class="fs12 " style="height: 240rpx;" v-model="submitData.description" placeholder="说几句,让更多小伙伴了解我" />
				</view>
			<view class="oh" style="display: flex;">
				<block v-for="(image,index) in submitData.bannerImg" :key="index">
					<view class="picRow">
						<image :src="item.url"></image>
					</view>
				</block>
				<view class="picRow" @click="upLoadImg('bannerImg')" v-if="submitData.bannerImg&&submitData.bannerImg.length<3">
					<image src="../../static/images/release-icon.png" mode=""></image></view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<view class="btn" @click="Utils.stopMultiClick(submit)">发表</view>
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
				wx_info:{},
				is_show:false,
				normalSrc: '../../static/images/details-icon.png',
				selectedSrc: '../../static/images/details-icon1.png',
				halfSrc: '../../static/images/details-icon1-a.png',
				submitData: {
					order_no: '',
					relation_id: '',
					score: 0,
					description: '',
					type:4,
					mainImg:[],
					relation_user:'',
					bannerImg:[]
				},
				stars: [1, 2, 3, 4, 5],
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self)
		},
		
		methods: {
			
				
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
							self.submitData.title=self.userInfoData.name;
							self.submitData.mainImg=self.userInfoData.mainImg;
						};
						self.$Utils.finishFunc('getUserInfoData');
					};
					self.$apis.userInfoGet(postData, callback);
				},
				
			  upLoadImg(type) {
			  	const self = this;	
			  	if (self.submitData[type].length > 2) {
			  		api.showToast('仅限3张', 'fail');
			  		return;
			  	};
			  	uni.showLoading({
			  		mask: true,
			  		title: '上传中',
			  	});
			  	const callback = (res) => {
			  		console.log('res', res)
			  		if (res.solely_code == 100000) {
			  			self.submitData[type].push({url:res.info.url,type:'image'})
			  			console.log('type',type)
			  			console.log(self.submitData)
			  			uni.hideLoading()
			  		} else {
			  			self.$Utils.showToast('网络故障', 'none')
			  		}
			  	};				
			  	uni.chooseImage({
			  		count: 3,
			  		success: function(res) {
			  			console.log(res);
			  			var tempFilePaths = res.tempFilePaths;
			  			console.log(callback)
			  			for (var i = 0; i < tempFilePaths.length; i++) {
			  				self.$Utils.uploadFile(tempFilePaths[i], 'file', {
			  					tokenFuncName: 'getUserToken'
			  				}, callback)
			  			}
			  		},
			  		fail: function(err) {
			  			uni.hideLoading();
			  		},			
			  	})			
			  },
			
			  select(key) {
				const self = this;
				self.submitData.score = key
			    console.log("得" + key + "分")
			  },
			
					
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
					shop:{
						token:uni.getStorageSync('user_token'),
						tableName:'UserInfo',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.submitData.order_no = self.mainData.order_no;
						
						self.submitData.relation_id = self.mainData.product_id
						self.submitData.relation_user = self.mainData.shop_no
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
					
				};
				self.$apis.orderGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.bannerImg;
				delete newObject.mainImg;
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
					self.messageAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [{
				  tableName:'Order',
				  FuncName:'update',
				  searchItem:{
				    id:self.id
				  },
				  data:{
				    isremark:1,
				  }
				}];
				const callback = (data) => {
			
					if (data.solely_code == 100000) {
			
						self.$Utils.showToast('评价成功', 'none')
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
			
				};
				self.$apis.messageAdd(postData, callback);
			},
		}
	};
</script>


<style>
	@import "../../assets/style/star.css";
	@import "../../assets/style/foodDetail.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.canTing{padding: 30rpx 4%;}
	.starBox image{width: 32rpx;height: 32rpx;margin-right: 8rpx;}
	textarea{padding:20rpx 0;}
	
	.picRow{margin: 0 20rpx 20rpx 0;float: left;width: 140rpx;height: 140rpx;background: #F5F5F5;}
	.picRow:nth-of-type(4n){margin-right: 0;}
	.picRow image{width: 100%;height: 100%; display: block;}
</style>
