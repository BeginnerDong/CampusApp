<template>
	<view>
		
		<view class="nipCard mglr4">
			<view class="item radius10 boxShaow">
				<view class=""><image class="topBaner" src="../../static/images/nipbanli-img.png" mode=""></image></view>
				<view class="infor flexRowBetween">
					<view class="flex">月卡<view class="fs16 red ftw mgl10"><text class="fs12">￥</text>10</view></view>
					<view class="chongBtn"  @click="paywayShow">充值</view>
				</view>
			</view>
			<view class="item radius10 boxShaow">
				<view class=""><image class="topBaner" src="../../static/images/nipbanli-img1.png" mode=""></image></view>
				<view class="infor flexRowBetween">
					<view class="flex">年卡<view class="fs16 red ftw mgl10"><text class="fs12">￥</text>100</view></view>
					<view class="chongBtn"  @click="paywayShow">充值</view>
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="paywayShow whiteBj" v-show="is_paywayShow">
			<view class="closebtn" @click="paywayShow">×</view>
			<view class="flexColumn center pdb25">
				<view class="center ftw fs18 mgb15">￥10</view>
				<view class="time color6 fs12">请在<text class="red">19:57</text>内完成支付</view>
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
				<button class="btn" type="button">立即支付</button>
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
				is_paywayShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
