<template>
	<view>
		
		<view class="mglr4">
			<view class="coupon pdlr4 radius10 whiteBj mgt15">
				<view class="item pdtb15 flex">
					<view class="ll mgr10"><image class="pic" :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image></view>
					<view class="rr fs11">
						<view class="fs14 color2 flexRowBetween">
							<view>{{mainData.title}}</view>
							<view class="color9 fs12">￥{{mainData.price}}</view>
						</view>
						<view class="color9 pdtb5">周一至周日 | 部分可用 | 免预约 | 可叠加2张</view>
						<view class="flex">
							<view class="tag flex"><image class="tagIcon" src="../../static/images/the-order-icon.png" mode=""></image><text>随时退</text></view>
							<view class="tag flex"><image class="tagIcon" src="../../static/images/the-order-icon.png" mode=""></image><text>过期自动退</text></view>
							<view class="tag flex"><image class="tagIcon" src="../../static/images/the-order-icon.png" mode=""></image><text>到店吃</text></view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="pdlr4 radius10 whiteBj mgt15">
				<view class="pdtb15 flexRowBetween borderB1">
					<view>数量</view>
					<view class="">
						<view class="numBox flex">
							<view class="btn" @click="counter('-')">-</view>
							<view class="num flexCenter">{{count}}</view>
							<view class="btn plus" @click="counter('+')">+</view>
						</view>
					</view>
				</view>
				<view class="pdtb15 flexRowBetween">
					<view>小计</view>
					<view class="red fs14">NIP: {{totalScore}}</view>
				</view>
			</view>
			
			<view class="pdlr4 radius10 whiteBj mgt15">
				<view class="pdtb15 flexRowBetween">
					<view>手机号码</view>
					<view style="width: 50%;">
						<input type="number" v-model="submitData.phone" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween">
			<view class="left fs12 color6">合计<text class="price fs16">{{totalScore}}<span style="font-size: 12px;" v-if="isFree">(会员免单一份)</span></text></view>
			<view class="payBtn pubBj white" @click="Utils.stopMultiClick(submit)">立即兑换</view>
		</view>
		
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="payShow whiteBj radius10 center" v-show="is_payShow">
			<view class="closebtn fs18" @click="payShow">×</view>
			<view class="fs16 color2 pdt5 avoidOverflow2">{{mainData.title}}</view>
			<view class="fs14 red mgt20">NIP:{{totalScore}}</view>
			<!-- <view class="flexCenter pdt20">
				<view class="time fs12 color6">请在<text class="red">19:57</text>内完成支付</view>
			</view> -->
			<view class="submitbtn" style="margin-top: 140rpx;">
				<button class="Wbtn" type="button" @click="goPay">立即兑换</button>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="paywayShow whiteBj" v-show="is_paywayShow">
			<view class="closebtn" @click="paywayShow">×</view>
			<view class="flexColumn center pdb25">
				<view class="center ftw fs18 mgb15">￥{{totalScore}}</view>
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
				Utils:this.$Utils,
				
				is_show:false,
				count:1,
				is_paywayShow:false,
				submitData:{
					shop_no:'',
					phone:''
				},
				mainData:{},
				totalScore:0,
				isFree:false,
				currPay:1,
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			seltPayway(currPay){
				const self = this
				if(currPay!=self.currPay){
					self.currPay = currPay
				}
			},
			
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
				var nowTime = Date.parse(new Date());
				self.totalScore = 0;
				self.totalScore = self.mainData.price * self.count;
				self.totalScore = parseFloat(self.totalScore).toFixed(2)
				if(parseInt(self.userInfoData.member_time)	>nowTime&&self.userInfoData.order.length==0){
					self.totalScore = parseFloat(self.totalScore - parseFloat(self.mainData.price)).toFixed(2);
					self.isFree = true
				};
			},
			
			paywayShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_paywayShow = !self.is_paywayShow;
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					order:{
						tableName:'Order',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							pay_status:1,
							product_id:self.mainData.id,
							free:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					};
					self.countTotalPrice()
					self.$Utils.finishFunc('getMainData');
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
						self.submitData.shop_no = self.mainData.shop_no;
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.getUserInfoData()
					
					console.log('self.mainData', self.mainData)
					
				};
				self.$apis.productGet(postData, callback);
			},
			
			submit(){
				const self = this;
				var nowTime = Date.parse(new Date());
				uni.setStorageSync('canClick',false);
				const pass = self.$Utils.checkComplete(self.submitData);
				if(!pass){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请补全信息','none')
				}else{
					if(parseFloat(self.userInfoData.score)<parseFloat(self.totalScore)){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('NIP不足','none');
						return
					}
					var data = self.$Utils.cloneForm(self.submitData);
					if(parseInt(self.userInfoData.member_time)>nowTime&&self.userInfoData.order.length==0){
						data.free = 1
					};
					var orderList = [
						{product_id:self.mainData.id,count:self.count,type:1,data:data}
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
										title: '兑换成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user-NIP-coupon/user-NIP-coupon'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '兑换失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '兑换成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user-NIP-coupon/user-NIP-coupon'}})
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
			
			/* goPay(order_id) {
				const self = this;	
				var nowTime = Date.parse(new Date());
				var postData = {};
				postData = {
					aliPay:{
						price:self.totalScore
					}
				};
				if(self.currPay==2){
					postData = {
						wxPay:{
							price:self.totalScore
						}
					}
				};
				postData.tokenFuncName = 'getUserToken',
				postData.searchItem = {
					id: self.orderId
				};	
				if(parseInt(self.userInfoData.member_time)>nowTime&&self.userInfoData.order.length==0){
					
					postData.other = {
						price:parseFloat(self.mainData.price)
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							if(res.info.package){
								var str=res.info.package
								var arr=str.split("=")[1]
								//胜利ar as1	=arr.toUpperCase()
								//console.log(as1)
								var obj={
									appid:res.info.appId,      //id 应用id
									partnerid:'1573719401',              //商户号 
									prepayid:arr,                         //预兑换
									package:'Sign=WXPay',
									noncestr:res.info.nonceStr,
									timestamp:res.info.timeStamp,   //时间戳
									sign:res.info.paySign.substring(0,30)
								}
								var orderInfo = JSON.stringify(obj);
							}else{
								var orderInfo = res.info
							}
							
							console.log(res.info);
							uni.requestPayment({
								
							    provider: self.currPay==1?'alipay':'wxpay',
							    orderInfo:orderInfo, //微信、支付宝订单数据
							    success: function (res) {
							        console.log('success:' + JSON.stringify(res));
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user-NIP-coupon/user-NIP-coupon'}})
									}, 1000);
							    },
							    fail: function (err) {
							        console.log('fail:' + JSON.stringify(err));
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
							    },
								complete: function (err) {
								    console.log('fail:' + JSON.stringify(err));
								},
							});
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user-NIP-coupon/user-NIP-coupon'}})
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
			}, */
		
		}
	};
</script>


<style>
	@import "../../assets/style/foodDetail.css";
	@import "../../assets/style/confirm.css";
	
	page{padding-bottom:150rpx;background: #F5F5F5;}
	input{width: 100%; line-height: 48rpx;display: block;box-sizing: border-box;font-size: 26rpx; color: #666;text-align: right;}
	.paywayShow{position: fixed;left: 0;right: 0;bottom: 0;padding: 40rpx 4% 80rpx 4%;z-index: 45;box-sizing: border-box;}
	.paywayShow .time{padding: 0 30rpx;line-height: 50rpx;background: #F5F5F5;border-radius: 20rpx;}
	.paywayShow .ll{width: 10%;}
	.paywayShow .Rselt{width: 90%;padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.payIcon{width: 40rpx;height: 40rpx;display: block;}
	
</style>
