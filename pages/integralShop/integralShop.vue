<template>
	<view>
		<view class="proLis flexRowBetween mglr4 pdt15">
			<view class="item-lis boxShaow" v-for="(item,index) in produtList" :key="index" @click="Router.navigateTo({route:{path:'/pages/integralShop-detail/integralShop-detail'}})">
				<view class="pic">
					<image src="../../static/images/integral-mall-img.png" alt="" />
				</view>
				<view class="infor">
					<view class="tit avoidOverflow fs13 pdb5">名称名称名称名称名称</view>
					<view class="red flex">
						<view class="fs11 mgr5">NIP:</view>
						<view class="ftw fs15">56.00</view>
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
				wx_info:{},
				is_show:false,
				produtList:[{},{},{},{},{},{}]
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
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	/* 商品列表 */
	.proLis{flex-wrap: wrap;}
	.proLis .item-lis{ width: 330rpx; overflow: hidden;background: #fff;margin-bottom: 30rpx; border-radius: 8rpx;}
	.proLis .item-lis .pic{width: 100%;height: 300rpx;display: block;margin: 0 auto;}
	.proLis .item-lis .pic image{width: 100%; height: 100%; display: block;}
	.proLis .item-lis .infor{padding: 20rpx 4%;}
</style>
