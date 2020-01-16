<template>
	<view>
		<view class="borderB1 flexEnd" style="padding: 30rpx 4%;">
			<view @click="Router.navigateTo({route:{path:'/pages/register/register'}})">快速登录/注册</view>
		</view>
		<view class="loginBox">
			<view class="fs18 ftw pdt30 pdb10">账号登录</view>
			<view class="items fs13 color6">
				<view class="item flex borderB1">
					<input type="number" maxlength="11" v-model="submitData.login_name" placeholder="手机号">
				</view>
				<view class="item flex borderB1 flexRowBetween">
					<view style="width: 50%;">
						<input type="text"  value="" placeholder="验证码">
					</view>
					<view class="fs14 ftw color2">获取验证码</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 240rpx;">
				<button class="Wbtn" type="button" @click="submit">确定</button>
			</view>
			
			<view class="wxLogin">
				<view class="fs12 color9 center">
					<view class="w flexCenter">
						<view class="xian"></view>
						<view class="pdl15 pdr15">微信登录</view>
						<view class="xian"></view>
					</view>
					<view class="mgt20"><image class="wxIcon" src="../../static/images/login-icon2.png" mode=""></image></view>
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
				submitData:{
					login_name:''
				}
			}
		},
		
		onLoad() {
			const self = this;
			uni.hideLoading()
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			submit() {
				const self = this;
				const postData = {
					login_name: self.submitData.login_name,
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('user_token', res.token);
							uni.setStorageSync('user_info', res.info);
							setTimeout(function() {
								self.Router.redirectTo({route:{path:'/pages/index/index'}})
							}, 1000);
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.loginByUser(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
		}
	};
</script>

<style>
	
	.loginBox{padding: 0 10%;}
	.items .item{padding: 50rpx 0 20rpx 0;border-bottom: 1px solid #eee;}
	input{width: 100%; display: block;box-sizing: border-box;font-size: 26rpx;}
	
	.wxLogin{position: fixed; bottom:50rpx;left: 0;right: 0;width: 100%;}
	.xian{width: 30%;height: 1px;background: #F5F5F5;}
	.wxIcon{width: 80rpx;height: 80rpx;border-radius: 50%;}
</style>
