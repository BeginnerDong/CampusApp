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
			<view class="orderList">
				<view class="item whiteBj radius10">
					<view class="flexRowBetween fs12">
						<view class="color9">交易时间：2019-1-3</view>
						<view class="red">已发货</view>
					</view>
					<view class="coupon pdtb10 borderB1 flex">
						<view class="ll mgr10"><image class="pic" src="../../static/images/integral-mall-img.png" mode=""></image></view>
						<view class="rr pr">
							<view class="pdt5 avoidOverflow2">双菇蒸饺</view>
							<view class="flexRowBetween B-price">
								<view class="red fs12">NIP:<text class="mgl5 ftw fs15">56.00</text></view>
								<view class="color9 fs13">×1</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdt15 fs12 center borderT1">
						<view class="B-Btn">确认收货</view>
					</view>
				</view>
				<view class="item whiteBj radius10">
					<view class="flexRowBetween fs12">
						<view class="color9">交易时间：2019-1-3</view>
						<view class="red">已收货</view>
					</view>
					<view class="coupon pdtb10 flex">
						<view class="ll mgr10"><image class="pic" src="../../static/images/integral-mall-img.png" mode=""></image></view>
						<view class="rr pr">
							<view class="pdt5 avoidOverflow2">双菇蒸饺</view>
							<view class="flexRowBetween B-price">
								<view class="red fs12">NIP:<text class="mgl5 ftw fs15">56.00</text></view>
								<view class="color9 fs13">×1</view>
							</view>
						</view>
					</view>
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
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			}
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
