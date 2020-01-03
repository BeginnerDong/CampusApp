<template>
	<view>
		<view class="">
			<view class="myRowBetween whiteBj">
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/userSetting-notice/userSetting-notice'}})" >
					<view class="ll">通知</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/userSetting-help/userSetting-help'}})" >
					<view class="ll">帮助与客服</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
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
	@import "../../assets/style/editInfor.css";
	page{background: #F5F5F5;}
	
	.myRowBetween .item{padding: 30rpx 4%;}
</style>
