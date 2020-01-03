<template>
	<view>
		
		<view class="detailBaner">
			<image src="../../static/images/goods-details-img.png" mode=""></image>
		</view>
		<view class="mglr4 pdtb10">
			<view class="fs14 pdb5" >芒果标题芒果标题500g</view>
			<view class="red fs15 ftw"><text class="ftn fs12 mgr5">NIP:</text>56.00</view>
		</view>	
		<view class="f5H5"></view>
		
		<view class="mglr4">
			<view class="pdt15 pdb10">商品详情</view>
			<view class="xqInfor">
				<view>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
				<view>
					<image src="../../static/images/goods-details-img1.png" mode="widthFix"></image>
				</view>
				<view>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 60rpx">
			<button class="btn" type="button"  @click="Router.navigateTo({route:{path:'/pages/integral-orderConfirm/integral-orderConfirm'}})">兑换</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	page{padding-bottom: 60rpx;}
	.detailBaner image{width: 100%;height: 360rpx; display: block;}
	.xqInfor view {font-size: 26rpx;color: #666;padding-bottom: 20rpx;}
	.xqInfor image{max-width: 100% !important; display: block;margin: 20rpx auto;}
</style>
