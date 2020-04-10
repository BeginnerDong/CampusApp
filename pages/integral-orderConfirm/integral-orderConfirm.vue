<template>
	<view>
		
		<view class="mglr4">
			<view class="coupon pdlr4 radius10 whiteBj mgt15">
				<view class="item pdtb15 flex">
					<view class="ll mgr10">
						<image class="pic" :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="rr fs11 pr">
						<view class="fs14 color2 pdt5 avoidOverflow2">{{mainData.title}}</view>
						<view class="red fs12 B-price">NIP:<text class="mgl5 ftw fs15">{{mainData.price}}</text></view>
					</view>
				</view>
			</view>
			
			
			
			<view class="pdlr4 radius10 whiteBj mgt15">
				<view class="pdtb15 flexRowBetween">
					<view>购买数量</view>
					<view class="">
						<view class="numBox flex">
							<view class="btn" @click="counter('-')">-</view>
							<view class="num flexCenter">{{count}}</view>
							<view class="btn plus" @click="counter('+')">+</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="radius10 whiteBj mgt15 editLine">
				<view class="item flexRowBetween">
					<view class="ll">姓名：</view>
					<view class="rr">
						<input type="text" v-model="submitData.name" placeholder="请输入姓名" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">电话：</view>
					<view class="rr">
						<input type="number" maxlength="11" v-model="submitData.phone" placeholder="请输入联系方式" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">地址：</view>
					<view class="rr">
						<input type="text" v-model="submitData.address" placeholder="请输入地址" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexEnd">
			<view class="left fs12 color6 mgr15">合计：<text class="red fs15">{{totalScore}}</text></view>
			<view class="payBtn pubBj white"  @click="Utils.stopMultiClick(submit)">立即兑换</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="payShow whiteBj radius10 center" v-show="is_payShow">
			<view class="closebtn fs18" @click="payShow">×</view>
			<view class="fs18">￥10</view>
			<view class="flexCenter pdt20">
				<view class="time fs12 color6">请在<text class="red">19:57</text>内完成支付</view>
			</view>
			<view class="submitbtn" style="margin-top: 140rpx;">
				<button class="Wbtn" type="button" @click="goPay">立即支付</button>
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
				
				is_show:false,
				count:1,
				is_payShow:false,
				submitData:{
					name:'',
					phone:'',
					address:''
				},
				mainData:{},
				totalScore:0
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		methods: {
			
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalScore = 0;
				self.totalScore = self.mainData.price * self.count;
				self.totalScore = parseFloat(self.totalScore).toFixed(2)
			},
			
			payShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_payShow = !self.is_payShow;
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				 const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.countTotalPrice()
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				const pass = self.$Utils.checkComplete(self.submitData);
				if(!pass){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请补全信息','none')
				}else{
					if(parseFloat(self.userInfoData.score)<parseFloat(self.totalScore)){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('积分不足','none');
						return
					}
					var data = self.$Utils.cloneForm(self.submitData)
					var orderList = [
						{product_id:self.mainData.id,count:self.count,type:2,data:data}
					];
					self.addOrder(orderList)
				}
			},
			
			addOrder(orderList) {
				const self = this;	
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.orderId = res.info.id;
						//self.goPay()
						self.is_show = !self.is_show;
						self.is_payShow = !self.is_payShow;
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
				const postData = {
					score:{
						price:self.totalScore
					}
				};
				postData.tokenFuncName = 'getUserToken',
				postData.searchItem = {
					id: self.orderId
				};	
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
										self.$Router.redirectTo({route:{path:'/pages/user-order/user-order'}})
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
								self.$Router.redirectTo({route:{path:'/pages/user-order/user-order'}})
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
		}
	};
</script>

<style>
	@import "../../assets/style/foodDetail.css";
	@import "../../assets/style/confirm.css";
	@import "../../assets/style/editInfor.css";
	
	page{padding-bottom:150rpx;background: #F5F5F5;}
	
	.coupon .pic{width: 180rpx;height: 180rpx;}
	.coupon .rr{width: 68%;height: 180rpx;box-sizing: border-box;}
	.B-price{position: absolute;left: 0;bottom: 10rpx;right: 0;}
	
	.editLine .item{padding: 30rpx 4%;}
	
	.xqbotomBar{padding: 0;}
	.xqbotomBar .payBtn{border-radius: 0;line-height: 100rpx;}
</style>
