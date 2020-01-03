<template>
	<view>
		<view class="myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">社区图像</view>
				<view class="rr">
					<view class="Rphoto mgr10" @click="Router.navigateTo({route:{path:'/pages/personHome-replaceImg/personHome-replaceImg'}})"><image src="../../static/images/about-img.png" mode=""></image></view>
					<view><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">社区名称</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item">
				<view>社区简介</view>
				<view class="pdt15">
					<textarea value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top:160rpx;">
			<button class="btn" type="button">提交</button>
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
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	.myRowBetween .item{padding: 30rpx 4%;}
	
	.Rphoto{width: 80rpx;height: 80rpx;background: #fff;padding: 4rpx;border-radius: 50%;border: 1px solid #eee; overflow: hidden;}
	.Rphoto image{width: 100%;height: 100%; display: block;}
	
	.myRowBetween .item textarea{height: 240rpx;background: #F5F5F5;border-radius: 10rpx;border: 0;}
</style>
