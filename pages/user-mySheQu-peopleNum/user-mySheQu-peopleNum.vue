<template>
	<view>
		
		<view class="whiteBj pdlr4 pdt15 pdb15">
			<view class="flex SQPerNum pdt10">
				<view class="photo" v-for="(item,index) in SQPerNum" :key="index" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/xiaoxi-img.png" mode=""></image></view>
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
				SQPerNum:[{},{},{},{},{},{},{},{},{},{},{},{},{},{},{}],
				userHomeDate:[{},{}]
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
	@import "../../assets/style/userHome.css";
	page{background: #F5F5F5;}
	.SQPerNum{flex-wrap: wrap;}
	.SQPerNum .photo{width: 80rpx;height: 80rpx;margin:0 40rpx 20rpx 0;}
	.SQPerNum .photo image{width: 100%;height: 100%;border-radius: 50%;}
	.SQPerNum .photo:nth-of-type(6n){margin-right: 0;}
</style>
