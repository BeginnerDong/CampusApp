<template>
	<view>
		<view class="orderNav flexRowBetween color6 whiteBj">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">未使用</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">已使用</view>
			<view class="tt" :class="num==3?'on':''" @click="change('3')">已评价</view>
		</view>
		
		<view class="mglr4">
			<view class="CouponCard" v-show="num==1">
				<view class="item"  v-for="(item,index) in mainData" :key="index">
					<view class="quanIcon">
						<image src="../../static/images/dianziquan-icon1.png" mode=""></image>
					</view>
					<view class="flexRowBetween">
						<view class="L-tit">
							<view class="fs10">{{item.title}}</view>
							<view class="fs16 pdt5 pdb10 ftw">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?
							item.orderItem[0].snap_product.description:''}}</view>
						</view>
						<view class="flexEnd">
							<view class="useBtn fs12"  @click="ewmShow(index)">立即使用</view>
						</view>
					</view>
					
					<view class="xian"></view>
					<view class="flexRowBetween pdt10 fs11">
						<view>{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?
							item.orderItem[0].snap_product.passage1:''}}</view>
						<view class="fs10 color9">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?
							item.orderItem[0].snap_product.passage2:''}}</view>
					</view>
				</view>
			</view>
		
			<view class="CouponCard" v-show="num==2">
				<view class="radius10 whiteBj" v-for="(item,index) in mainData" :key="index">
					<view class="item">
						<view class="quanIcon"><image src="../../static/images/dianziquan-icon1.png" mode=""></image></view>
						<view class="fxIcon"><image src="../../static/images/dianziquan-icon.png" mode=""></image></view>
						<view class="flexRowBetween">
							<view class="L-tit">
								<view class="fs10">{{item.title}}</view>
								<view class="fs16 pdt5 pdb10 ftw">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?
							item.orderItem[0].snap_product.description:''}}</view>
							</view>
						</view>
						
						<view class="xian"></view>
						<view class="flexRowBetween pdt10 fs11">
							<view>{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?
							item.orderItem[0].snap_product.passage1:''}}</view>
							<view class="fs10 color9">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?
							item.orderItem[0].snap_product.passage2:''}}</view>
						</view>
					</view>
					<view class="flexEnd pdtb15 mglr4 fs12 center">
						<view class="B-Btn" :data-id="item.id" 
						@click="Router.navigateTo({route:{path:'/pages/user-NIP-couponPingJia/user-NIP-couponPingJia?id='+$event.currentTarget.dataset.id}})">去评价</view>
					</view>
				</view>
			</view>
			<view class="radius10 whiteBj mgt15 canTing"  v-show="num==3">
				<view class="item pr"  v-for="(item,index) in mainData" :key="index">
					<!-- <view class="quanIcon">
						<image src="../../static/images/dianziquan-icon1.png" mode=""></image>
					</view> -->
					<view class="flex">
						<view class="pic">
							<image :src="item.shop&&item.shop[0]&&item.shop[0].mainImg
							&&item.shop[0].mainImg[0]?item.shop[0].mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor mgl10  fs12 color6" style="width: 70%;">
							<view class="fs14 color2 ftw">{{item.shop&&item.shop[0]?item.shop[0].name:''}}</view>
							<!-- <view class="flex" style="flex-wrap: wrap;">
								<view class="lable">甜点</view>
							</view> -->
							<view class="adrs mgt15">{{item.shop&&item.shop[0]?item.shop[0].address:''}}</view>
						</view>
					</view>
					<view class="color6 fs12 pjtex">{{item.message&&item.message[0]?item.message[0].description:''}}</view>
				</view>
			</view>
			
		</view>
		
		<view class="black-bj" v-if="is_show"></view>
		<view class="ewmShow whiteBj radius10" v-if="is_ewmShow">
			<view class="closebtn" @click="ewmShowFalse()">×</view>
			<view><image class="ewmImg" 
			:src="mainData[index].qrcode" mode="widthFix"></image>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show:false,
				num:1,
				couponData:[{},{}],
				is_ewmShow:false,
				mainData:[],
				searchItem:{
					type:1,
					pay_status:1,
					transport_status:0
				},
				index:-1
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			
		},
		
		onShow() {
			const self = this;
			self.getMainData(true)
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
			
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
					if(self.num==1){
						self.searchItem.transport_status =0;
						self.searchItem.isremark =0;
					}else if(self.num==2){
						self.searchItem.transport_status =2;
						self.searchItem.isremark =0;
					}else if(self.num==3){
						self.searchItem.transport_status =2;
						self.searchItem.isremark =1;
					};
					
					self.getMainData(true)
					
				}
			},
			
			ewmShow(index){
				const self = this;
				self.index = index;
				self.is_show = !self.is_show;
				self.is_ewmShow = !self.is_ewmShow;
			},
			
			ewmShowFalse(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_ewmShow = !self.is_ewmShow;
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
				postData.tokenFuncName = 'getUserToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
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
					message:{
						tableName:'Message',
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
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/foodDetail.css";
	page{background: #F5F5F5;}
	.orderNav .tt{width: 33.3%;}
	
	.CouponCard .L-tit{width: 66%;}
	.CouponCard .useBtn{width: 140rpx;line-height: 40rpx;border-radius: 30rpx;border: 1px solid #FF3B3B; color: #F00;text-align: center;}
	.quanIcon{width: 60rpx;height: 57rpx; position: absolute; top: 0; right: 0;}
	.quanIcon image{width: 100%;height: 100%; display: block;}
	
	.ewmShow{width: 75%;padding: 120rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 42;box-sizing: border-box;}
	.ewmShow .ewmImg{width: 100%;display: block;}
	.B-Btn{width: 140rpx;border-radius: 10rpx;border:1px solid #eee;line-height: 50rpx;}
	
	.canTing .item{padding: 30rpx 4%;}
	.canTing .item .pjtex{padding: 30rpx;margin-top: 30rpx;background: #F5F5F5;line-height: 36rpx;border-radius: 10rpx;}
</style>
