<template>
	<view>
		<view class="pdtb15 borderB1 mglr4">
			<textarea value="" placeholder="请输入您对此平台的意见" placeholder-class="placeholder" />
		</view>
		
		<view class="submitbtn" style="margin-top:200rpx;">
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
	page{padding-bottom: 60rpx;}
	
	textarea{height: 600rpx;border: 0;padding: 0;}
</style>
