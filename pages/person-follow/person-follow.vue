<template>
	<view>
		
		<view class="myRowBetween pdlr4">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index" >
				<view class="ll flex" :data-user_no ="item.user_no"
						@click="Router.navigateTo({route:{path:'/pages/userHome/userHome?user_no='+$event.currentTarget.dataset.user_no}})">
					<view class="photo">
						<image :src="item.mainImg&&item.mainImg.length>0?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						
					</view>
					<view class="ll-tit avoidOverflow">{{item.name}}</view>
				</view>
				<view class="rr">
					<view class="joinOK flexCenter fs12" v-if="item.log&&item.log.length>0" 
					 @click="submit(index)">
						<!-- <image class="gzIcon" src="../../static/images/jia.png" mode=""></image> -->
						<text>取消关注</text>
					</view>
					<view class="joinOK flexCenter fs12" v-if="item.log&&item.log.length==0"
					 @click="submit(index)">
						<image class="gzIcon" src="../../static/images/jia.png" mode=""></image>
						<text>关注</text>
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
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					user_type:0
				},
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
							relation_user:['in',[uni.getStorageSync('user_info').user_no]],
							user_type:['in',0]
						},
						middleKey: 'user_no',
						key: 'user_no',
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
			
			submit(index) {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (self.mainData[index].log.length == 0) {
					self.addLog(index)
				} else {
					self.updateLog(index)
				};
			},
			
			addLog(index) {
				const self = this;
				const postData = {};
				postData.data = {
					type: 5,
					title: '关注成功',
					relation_user: self.mainData[index].user_no,
					relation_table:'User',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.$Utils.showToast('关注成功', 'none', 1000)
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateLog(index) {
				const self = this;
			
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
						self.userInfoData.log[0].status = -self.userInfoData.log[0].status;
						if(self.userInfoData.log[0].status==1){
							self.$Utils.showToast('关注成功', 'none', 1000)
						}else{
							self.$Utils.showToast('取消成功', 'none', 1000)
						}
						setTimeout(function() {
							self.getMainData(true)
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
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/alertBox.css";
	
	.myRowBetween .item .ll{width: 70%;}
	.myRowBetween .item .rr{width: 30%;}
	.joinOK{padding: 0 10rpx;width: 140rpx;text-align: center;line-height: 50rpx;border-radius: 30rpx;border: 1px solid #eee;box-sizing: border-box;}
	.joinOK .gzIcon{width: 22rpx;height: 22rpx;display: block;margin-right: 10rpx;}
</style>
