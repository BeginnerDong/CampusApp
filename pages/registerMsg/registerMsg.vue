<template>
	<view>
		
		<view class="registerBox">
			<view class="items fs13 color6">
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon.png" mode=""></image></view>
					<view class="rr">
						<input type="text" maxlength="11" value="" placeholder="昵称">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon1.png" mode=""></image></view>
					<view class="rr">
						<input type="text" value="" placeholder="性别">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon2.png" mode=""></image></view>
					<view class="rr">
						<input type="text" value="" placeholder="学校">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon3.png" mode=""></image></view>
					<view class="rr">
						<input type="number" maxlength="11" value="" placeholder="入学时间">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon4.png" mode=""></image></view>
					<view class="rr">
						<input type="number" maxlength="11" value="" placeholder="学历">
					</view>
				</view>
			</view>
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="Wbtn" type="button" @click="Router.navigateTo({route:{path:'/pages/login/login'}})">提交</button>
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
				is_show:false
			}
		},
		onLoad() {
			const self = this;
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
	
	.registerBox{padding: 0 10%;}
	.items .item{padding: 50rpx 0 20rpx 0;border-bottom: 1px solid #eee; align-items: flex-start;}
	.items .item .icon{padding: 0 30rpx;width: 15%;box-sizing: border-box;}
	.items .item .icon image{width: 32rpx;height: 32rpx;display: block;}
	input{width: 100%; display: block;box-sizing: border-box;font-size: 26rpx;}
	.items .item .rr{width: 85%;}
	
</style>
