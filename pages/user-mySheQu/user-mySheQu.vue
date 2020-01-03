<template>
	<view>
		
		<view class="myRowBetween pdlr4">
			<view class="item flexRowBetween" v-for="(item,index) in followData" :key="index"  @click="Router.navigateTo({route:{path:'/pages/user-mySheQu-Home/user-mySheQu-Home'}})">
				<view class="ll flex">
					<view class="photo"><image src="../../static/images/xiaoxi-img.png" mode=""></image></view>
					<view class="ll-tit avoidOverflow">考研社区</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
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
				is_alertBox:false,
				followData:[{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			alertBoxShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_alertBox = !self.is_alertBox
			}
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/alertBox.css";
	
	.myRowBetween .item .ll{width: 70%;}
	.myRowBetween .item .rr{width: 30%;}
	.joinOK{padding: 0 10rpx;width: 140rpx;text-align: center;line-height: 50rpx;border-radius: 30rpx;border: 1px solid #eee;box-sizing: border-box;}
	.joinOK .okIcon{width: 21rpx;height: 15rpx;display: block;margin-right: 10rpx;}
</style>
