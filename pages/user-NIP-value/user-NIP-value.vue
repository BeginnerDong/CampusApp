<template>
	<view>
		<view class="myExtendTop center">
			<view class="flexCenter"><image class="icon" src="../../static/images/nipzhi-icon.png" mode=""></image></view>
			<view class="fs15 pdtb15">可用NIP值</view>
			<view class="money">￥1230</view>
			
		</view>
		<view class="f5H10"></view>
		<view class="orderNav flexRowBetween borderB1 color6">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">花费</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">获得</view>
			
		</view>
		
		<view class="">
			<view class="myRowBetween mglr4" v-show="num==1">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll">
						<view class="fs26">购买绿抹茶代金券</view>
						<view class="fs12 color6">2019.11.20</view>
					</view>
					<view class="rr">-85</view>
				</view>
			</view>
			<view class="myRowBetween mglr4" v-show="num==2">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll">
						<view class="fs26">签到</view>
						<view class="fs12 color6">2019.11.20</view>
					</view>
					<view class="rr">+185</view>
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
				num:1,
				spendData:[{},{},{}]
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
			}
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
