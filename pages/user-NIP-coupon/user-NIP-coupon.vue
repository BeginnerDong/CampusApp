<template>
	<view>
		<view class="orderNav flexRowBetween color6 whiteBj">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">未使用</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">已使用</view>
			<view class="tt" :class="num==3?'on':''" @click="change('3')">已评价</view>
		</view>
		
		<view class="mglr4">
			<view class="CouponCard" v-show="num==1">
				<view class="item"  v-for="(item,index) in couponData" :key="index">
					<view class="quanIcon"><image src="../../static/images/dianziquan-icon1.png" mode=""></image></view>
					<view class="flexRowBetween">
						<view class="L-tit">
							<view class="fs10">绿茶莉餐厅代金券</view>
							<view class="fs16 pdt5 pdb10 ftw">85元代100元券</view>
						</view>
						<view class="flexEnd">
							<view class="useBtn fs12"  @click="ewmShow">立即使用</view>
						</view>
					</view>
					
					<view class="xian"></view>
					<view class="flexRowBetween pdt10 fs11">
						<view>周一至周天可用</view>
						<view class="fs10 color9">10:00-22:00</view>
					</view>
				</view>
			</view>
		
			<view class="CouponCard" v-show="num==2">
				<view class="radius10 whiteBj" v-for="(item,index) in couponData" :key="index">
					<view class="item">
						<view class="quanIcon"><image src="../../static/images/dianziquan-icon1.png" mode=""></image></view>
						<view class="fxIcon"><image src="../../static/images/dianziquan-icon.png" mode=""></image></view>
						<view class="flexRowBetween">
							<view class="L-tit">
								<view class="fs10">绿茶莉餐厅代金券</view>
								<view class="fs16 pdt5 pdb10 ftw">85元代100元券</view>
							</view>
						</view>
						
						<view class="xian"></view>
						<view class="flexRowBetween pdt10 fs11">
							<view>周一至周天可用</view>
							<view class="fs10 color9">半年销量125</view>
						</view>
					</view>
					<view class="flexEnd pdtb15 mglr4 fs12 center">
						<view class="B-Btn" @click="Router.navigateTo({route:{path:'/pages/user-NIP-couponPingJia/user-NIP-couponPingJia'}})" >去评价</view>
					</view>
				</view>
			</view>
			<view class="radius10 whiteBj mgt15 canTing"  v-show="num==3">
				<view class="item pr"  v-for="(item,index) in couponData" :key="index">
					<view class="quanIcon"><image src="../../static/images/dianziquan-icon1.png" mode=""></image></view>
					<view class="flex">
						<view class="pic">
							<image src="../../static/images/coupons-img.png" mode=""></image>
						</view>
						<view class="infor mgl10  fs12 color6" style="width: 70%;">
							<view class="fs14 color2 ftw">绿茉莉</view>
							<view class="flex" style="flex-wrap: wrap;">
								<view class="lable">甜点</view>
							</view>
							<view class="adrs mgt15">南稍门 5.1km</view>
						</view>
					</view>
					<view class="color6 fs12 pjtex">发货人时间看了发货人后尽快孵化基地扣三分费塑料发黄发个低功耗供货方炬发VR和我恢复大师</view>
				</view>
			</view>
			
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="ewmShow whiteBj radius10" v-show="is_ewmShow">
			<view class="closebtn" @click="ewmShow">×</view>
			<view><image class="ewmImg" src="../../static/images/canyuhuodong-img1.png" mode="widthFix"></image></view>
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
				num:1,
				couponData:[{},{}],
				is_ewmShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
				}
			},
			ewmShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_ewmShow = !self.is_ewmShow;
			}
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
