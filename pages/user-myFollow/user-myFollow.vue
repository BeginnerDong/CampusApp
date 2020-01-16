<template>
	<view>
		
		<view class="myRowBetween pdlr4">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index" >
				<view class="ll flex" @click="Router.navigateTo({route:{path:'/pages/TA-Home/TA-Home'}})">
					<view class="photo">
						<image :src="item.mainImg&&item.mainImg.length>0?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						
					</view>
					<view class="ll-tit avoidOverflow">{{item.name}}</view>
				</view>
				<view class="rr">
					<view class="joinOK flexCenter fs12"  @click="updateLog(index)">
						<!-- <image class="gzIcon" src="../../static/images/jia.png" mode=""></image> -->
						<text>取消关注</text>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 加入提示弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="alertBox center pdt20 radius10 whiteBj" v-show="is_alertBox">
			<view>提示：</view>
			<view class="pdt10 pdb20 color6 fs13">确定不再关注？</view>
			<view class="flexRowBetween selt seltBtn fs13">
				<view class="half" @click="alertBoxShow">取消</view>
				<view class="half">确定</view>
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
				is_alertBox:false,
				followData:[{},{},{},{}],
				searchItem:{
					thirdapp_id:2,
					user_type:0
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
			
			alertBoxShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_alertBox = !self.is_alertBox
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
							relation_table: ['in', ['User']],
							type:['in',[5]],
							user_no:['in',[uni.getStorageSync('user_info').user_no]]
						},
						middleKey: 'user_no',
						key: 'relation_user',
						condition: 'in',
					},
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					log: {
						tableName: 'Log',
						searchItem: {
							relation_table: 'User',
							type:5,
							user_no:uni.getStorageSync('user_info').user_no
						},
						middleKey: 'user_no',
						key: 'relation_user',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data);
					
					}
			
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			updateLog(index) {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确定不再关注？',
					showCancel:true,
					success: function(res) {
						if (res.confirm) {
							const postData = {
								searchItem: {
									id: self.mainData[index].log[0].id
								},
								data: {
									status: -self.mainData[index].log[0].status
								}
							};
							postData.tokenFuncName = 'getUserToken';
							const callback = (res) => {
								uni.setStorageSync('canClick', true);
								if (res.solely_code == 100000) {
									self.mainData[index].log[0].status = -self.mainData[index].log[0].status;
									if(self.mainData[index].log[0].status==1){
										self.$Utils.showToast('关注成功', 'none', 1000)
									}else{
										self.$Utils.showToast('取消成功', 'none', 1000)
									}
									
								} else {
									self.$Utils.showToast(res.msg, 'none', 1000)
								};
							};
							self.$apis.logUpdate(postData, callback);
						} else if (res.cancel) {
							uni.setStorageSync('canClick', true);	
							console.log('用户点击取消');
						}
					}
				});
				
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/alertBox.css";
	
	.myRowBetween .item .ll{width: 70%;}
	.myRowBetween .item .rr{width: 30%;}
	.joinOK{padding: 0 10rpx;width: 140rpx;text-align: center;line-height: 50rpx;border-radius: 30rpx;border: 1px solid #eee;box-sizing: border-box;}
	.joinOK .gzIcon{width: 22rpx;height: 22rpx;display: block;margin-right: 10rpx;}
</style>
