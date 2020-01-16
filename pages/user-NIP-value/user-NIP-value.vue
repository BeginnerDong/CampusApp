<template>
	<view>
		<view class="myExtendTop center">
			<view class="flexCenter"><image class="icon" src="../../static/images/nipzhi-icon.png" mode=""></image></view>
			<view class="fs15 pdtb15">可用NIP值</view>
			<view class="money">￥{{userInfoData.score}}</view>
			
		</view>
		<view class="f5H10"></view>
		<view class="orderNav flexRowBetween borderB1 color6">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">花费</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">获得</view>
			
		</view>
		
		<view class="">
			<view class="myRowBetween mglr4" v-show="num==1">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="ll">
						<view class="fs26">购买{{item.order&&item.order[0]?item.order[0].title:''}}</view>
						<view class="fs12 color6">{{item.create_time}}</view>
					</view>
					<view class="rr">{{item.count}}</view>
				</view>
			</view>
			<view class="myRowBetween mglr4" v-show="num==2">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="ll">
						<view class="fs26">{{item.trade_info}}</view>
						<view class="fs12 color6">{{item.create_time}}</view>
					</view>
					<view class="rr">{{item.count}}</view>
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
				num:1,
				spendData:[{},{},{}],
				mainData:[],
				userInfoData:{},
				searchItem:{
					type:3,
					thirdapp_id:2,
					count:['<',0]
				},
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		methods: {
			
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num;
					if(self.num==1){
						self.searchItem.count = ['<',0]
					}else if(self.num==2){
						self.searchItem.count = ['>',0]
					};
					self.getMainData(true)
				}
			},
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getUserToken'
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no		
				
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.tokenFuncName = 'getUserToken';
				postData.getAfter ={
					order:{
						tableName:'Order',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/orderNav.css";
	
	.myExtendTop{padding: 80rpx 4%;}
	.myExtendTop .icon{width: 72rpx; height: 62rpx; display: block;}
	.myExtendTop .money{font-size: 50rpx; line-height:50rpx;}
</style>
