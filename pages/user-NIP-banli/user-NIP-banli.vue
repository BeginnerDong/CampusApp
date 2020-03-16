<template>
	<view>
		
		<view class="nipCard mglr4" v-if="mainData.length>0">
			<view class="item radius10 boxShaow" v-for="(item,index) in mainData">
				<view class=""><image class="topBaner" 
				:src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
				</view>
				<view class="infor flexRowBetween">
					<view class="flex">{{item.title}}<view class="fs16 red ftw mgl10"><text class="fs12">￥</text>{{item.price}}</view></view>
					<view class="chongBtn"  @click="submit(index)">充值</view>
				</view>
			</view>
			<!-- <view class="item radius10 boxShaow">
				<view class=""><image class="topBaner" src="../../static/images/nipbanli-img1.png" mode=""></image></view>
				<view class="infor flexRowBetween">
					<view class="flex">年卡<view class="fs16 red ftw mgl10"><text class="fs12">￥</text>100</view></view>
					<view class="chongBtn"  @click="paywayShow">充值</view>
				</view>
			</view> -->
		</view>
		<view v-else>
			<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="paywayShow whiteBj" v-show="is_paywayShow">
			<view class="closebtn" @click="paywayShow">×</view>
			<view class="flexColumn center pdb25">
				<view class="center ftw fs18 mgb15">￥{{chooseCard.price}}</view>
				<!-- <view class="time color6 fs12">请在<text class="red">19:57</text>内完成支付</view> -->
			</view>
			<view class="flexRowBetween" @click="seltPayway('1')">
				<view class="ll"><image class="payIcon" src="../../static/images/nipbanli-icon4.png" mode=""></image></view>
				<view class="Rselt flexRowBetween">
					<view class="">支付宝支付</view>
					<view><image class="seltIcon" :src="currPay==1?'../../static/images/nipbanli-icon.png':'../../static/images/nipbanli-icon1.png'" mode=""></image></view>
				</view>
			</view>
			<view class="flexRowBetween" @click="seltPayway('2')">
				<view class="ll"><image class="payIcon" src="../../static/images/nipbanli-icon3.png" mode=""></image></view>
				<view class="Rselt flexRowBetween">
					<view class="">微信支付</view>
					<view><image class="seltIcon" :src="currPay==2?'../../static/images/nipbanli-icon.png':'../../static/images/nipbanli-icon1.png'" mode=""></image></view>
				</view>
			</view>
			<view class="submitbtn mgt30">
				<button class="btn" type="button" @click="goPay">立即支付</button>
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
				currPay:1,
				is_paywayShow:false,
				searchItem:{
					type:3,
					thirdapp_id:2,
					on_shelf:1
				},
				mainData:[],
				chooseCard:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		methods: {
			
			getMainData(isNew) {
				const self = this;
				
				const postData = {};
				
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
			
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
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
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			
			
			
			submit(index){
				const self = this;
				uni.setStorageSync('canClick',false);
				self.chooseCard = self.mainData[index];
				if(parseFloat(self.userInfoData.score)<parseFloat(self.chooseCard.price)){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('积分不足','none');
					return
				}
				var orderList = [
					{product_id:self.chooseCard.id,count:1,type:3}
				];
				self.addOrder(orderList)
				
			},
			
			addOrder(orderList) {
				const self = this;	
				if(self.orderId){
					self.goPay()
					return
				};
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {};
				postData.tokenFuncName = 'getUserToken';
				
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.orderId = res.info.id;
						//self.goPay()
						self.is_show = !self.is_show
						self.is_paywayShow = !self.is_paywayShow
					} else {		
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay(order_id) {
				const self = this;	
				var nowTime = Date.parse(new Date());
				
				const postData = {
					score:{
						price:self.chooseCard.price
					}
				};
				postData.tokenFuncName = 'getUserToken',
				postData.searchItem = {
					id: self.orderId
				};	
				if(parseInt(self.userInfoData.member_time)>nowTime){
					self.time = parseInt(self.userInfoData.member_time) + parseInt(self.chooseCard.duration)
				}else{
					self.time = nowTime + parseInt(self.chooseCard.duration);
				};
				postData.payAfter = [{
					tableName: 'UserInfo',
					FuncName: 'update',
					data: {
						member_time: self.time
					},
					searchItem:{
						user_no:uni.getStorageSync('user_info').user_no
					}
				}, ];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user_myorder/user_myorder'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user_myorder/user_myorder'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			paywayShow(){
				const self = this
				self.is_show = !self.is_show
				self.is_paywayShow = !self.is_paywayShow
			},
			seltPayway(currPay){
				const self = this
				if(currPay!=self.currPay){
					self.currPay = currPay
				}
			}
		}
	};
</script>

<style>
	page{padding-bottom: 60rpx;}
	.nipCard .item{overflow: hidden;background: #fff;margin-top: 30rpx;}
	.nipCard .item .topBaner{width: 100%;height:360rpx;display: block;}
	.nipCard .item .infor{padding: 30rpx 4%;}
	.chongBtn{width: 100rpx;line-height: 60rpx;border-radius: 30rpx;background: #222;text-align: center;color: #fff;font-size: 26rpx;}
	
	.paywayShow{position: fixed;left: 0;right: 0;bottom: 0;padding: 40rpx 4% 80rpx 4%;z-index: 45;box-sizing: border-box;}
	.paywayShow .time{padding: 0 30rpx;line-height: 50rpx;background: #F5F5F5;border-radius: 20rpx;}
	.paywayShow .ll{width: 10%;}
	.paywayShow .Rselt{width: 90%;padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.payIcon{width: 40rpx;height: 40rpx;display: block;}

</style>
