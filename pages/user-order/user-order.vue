<template>
	<view>
		<view class="orderNav flexRowBetween whiteBj color6 W-Fixed borderB1" style="top: 88rpx;">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">全部</view>
			<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">待发货</view>
			<view class="tt flexCenter" :class="curr==3?'on':''" @click="changeCurr('3')">已发货</view>
			<view class="tt flexCenter" :class="curr==4?'on':''" @click="changeCurr('4')">已收货</view>
		</view>
		<view style="height: 82rpx;"></view>
		
		<view class="mglr4">
			<view class="orderList" v-if="mainData.length>0">
				<view class="item whiteBj radius10" v-for="(item,index) in mainData">
					<view class="flexRowBetween fs12">
						<view class="color9">交易时间：{{item.create_time}}</view>
						<view class="red" v-if="item.transport_status==0">待发货</view>
						<view class="red" v-if="item.transport_status==1">已发货</view>
						<view class="red" v-if="item.transport_status==2">已收货</view>
					</view>
					<view class="coupon pdtb10 borderB1 flex">
						<view class="ll mgr10">
							<image class="pic" :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
							item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?
							item.orderItem[0].snap_product.mainImg[0].url:''"></image>
						</view>
						<view class="rr pr">
							<view class="pdt5 avoidOverflow2">{{item.title}}</view>
							<view class="flexRowBetween B-price">
								<view class="red fs12">NIP:<text class="mgl5 ftw fs15">{{item.unit_price}}</text></view>
								<view class="color9 fs13">×{{item.count}}</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdt15 fs12 center borderT1" @click="orderUpdate(index)" v-if="item.transport_status==1">
						<view class="B-Btn">确认收货</view>
					</view>
				</view>
			</view>
			<view v-else>
				<view class="noDataBox"><image src="../../static/images/nodata.png" mode=""></image></view>
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
				curr:1,
				searchItem:{
					type:2,
					pay_status:1
				},
				mainData:[],
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
		
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
					if(self.curr==1){
						delete self.searchItem.transport_status
					}else if(self.curr==2){
						self.searchItem.transport_status = 0
					}else if(self.curr==3){
						self.searchItem.transport_status = 1
					}else if(self.curr==4){
						self.searchItem.transport_status = 2
					};
					self.getMainData(true)
				}
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
			
			orderUpdate(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					transport_status:2,
				};
				postData.searchItem = {
					id:self.mainData[index].id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	
	.orderList .item{padding: 30rpx 4%; margin-top: 30rpx;}
	
	.coupon .pic{width: 180rpx;height: 164rpx; display: block;}
	.coupon .rr{width: 68%;height: 164rpx;box-sizing: border-box;}
	.B-price{position: absolute;left: 0;bottom: 10rpx;right: 0;}
	.B-Btn{width: 140rpx;border-radius: 10rpx;border:1px solid #eee;line-height: 50rpx;}
	.borderT1{border-top: 1px solid #eee;}
</style>
