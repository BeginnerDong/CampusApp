<template>
	<view>
		<view class="">
			<view class="myRowBetween whiteBj">
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-NIP-SignIn/user-NIP-SignIn'}})" >
					<view class="ll">签到</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-NIP-value/user-NIP-value'}})" >
					<view class="ll">我的NIP值</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-NIP-coupon/user-NIP-coupon'}})" >
					<view class="ll">优惠券</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-NIP-banli/user-NIP-banli'}})" >
					<view class="ll">NIP办理</view>
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
