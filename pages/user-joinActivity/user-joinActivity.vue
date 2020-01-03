<template>
	<view>
		<view class="joinActive mglr4">
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail'}})">
				<view class="pdb10">活动标题活动标题活动标题活动标题</view>
				<view class="quanCard fs15">
					<view class="center fs22 ftw quanTit pdb15">入场券</view>
					<view>时间：2019.12.26 18:30</view>
					<view class="pdt10">地点：体育馆</view>
				</view>
				<view class="flexRowBetween pdt15 fs13">
					<view>发布者：</view>
					<view class="flexEnd">
						<view><image class="photo" src="../../static/images/home-img.png" mode=""></image></view>
						<view class="mgl10">雨水斗：15689568956</view>
					</view>
				</view>
			</view>
			<view class="item">
				<view class="overdue"><image src="../../static/images/overdue.png" mode=""></image></view>
				<view class="pdb10">活动标题活动标题活动标题活动标题</view>
				<view class="quanCard fs15">
					<view class="center fs22 ftw quanTit pdb15">入场券</view>
					<view>时间：2019.12.26 18:30</view>
					<view class="pdt10">地点：体育馆</view>
				</view>
				<view class="flexRowBetween pdt15 fs13">
					<view>发布者：</view>
					<view class="flexEnd">
						<view><image class="photo" src="../../static/images/home-img.png" mode=""></image></view>
						<view class="mgl10">雨水斗：15689568956</view>
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
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		}
	};
</script>

<style>
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	.joinActive .item{padding: 30rpx;background: #fff;border-radius: 10rpx;margin-top: 30rpx;position: relative;}
	.quanCard{width: 100%;height: 316rpx;padding: 50rpx 30rpx 30rpx 210rpx;background: url(../../static/images/canyuhuodong-img.png) no-repeat 0 0/100% 100%;box-sizing: border-box;}
	.joinActive .item .photo{width: 70rpx;height: 70rpx;border-radius: 50%;}
	.overdue{width: 100rpx; height: 100rpx;position: absolute;top: 0;right: 0;}
	.overdue image{width: 100%;height: 100%; display: block}
</style>
