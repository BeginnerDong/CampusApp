<template>
	<view>
		<view class="borderB1 flexEnd" style="padding: 30rpx 4%;margin-top: 44px;">
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
						<input type="text" v-model="submitData.code" placeholder="验证码">
					</view>
					<view class="fs14 ftw color2" @click="sendCode()" v-if="!hasSend">{{text}}</view>
					<view class="fs14 ftw color2"  v-else>{{text}}</view>
				</view>
			</view>

			<view class="submitbtn" style="margin-top: 240rpx;">
				<button class="Wbtn" type="button" @click="submit">确定</button>
			</view>

			<view class="wxLogin">
				<view class="fs12 color9 center" @click="wxLogin">
					<view class="w flexCenter">
						<view class="xian"></view>
						<view class="pdl15 pdr15">微信登录</view>
						<view class="xian"></view>
					</view>
					<view class="mgt20">
						<image class="wxIcon" src="../../static/images/login-icon2.png" mode=""></image>
					</view>
				</view>
			</view>
		</view>


	</view>

</template>

<script>
	import config from "@/config/index.config.js";
	export default {
		data() {
			return {
				Router: this.$Router,
				submitData: {
					login_name: '',
					code:''
				},
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
			}
		},

		onLoad() {
			const self = this;
			uni.hideLoading()
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			
			tokenGet() {
				const self = this;
				const postData = {
					searchItem: {
						user_no: 'U716837043691036'
					}
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.userData = res.info;
						uni.setStorageSync('user_token', res.token);
						uni.setStorageSync('user_info', res.info);
						var time = parseInt(new Date().getTime()) + 3500000;
						uni.setStorageSync('token_expire_time',time);
						self.Router.redirectTo({
							route: {
								path: '/pages/index/index'
							}
						})
					}
					console.log('res', res)
					self.$Utils.finishFunc('tokenGet');
				};
				self.$apis.tokenGet(postData, callback);
			},
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.login_name;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					
					return;
				}
				var postData = {
					data:{
						phone:self.submitData.login_name,
						type:2
					}
				};
				var callback = function(res){
					if(res.solely_code==100000){
						uni.setStorageSync('canClick',true);
						uni.hideLoading();
						self.hasSend = true;
						var interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
						
							self.text=self.currentTime + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(interval)
								
								self.hasSend = false;
								self.text='重新发送';
								self.currentTime= 61;
								
							}
							
						}, 1000);
					}else{
						uni.hideLoading();
						self.$Utils.showToast('发送失败', 'none', 1000)
					};
				};
				self.$apis.codeGet(postData, callback);
			},

			wxLogin() {
				const self = this;
				uni.login({
					provider: 'weixin',
					success: function(loginRes) {
						console.log(loginRes);
						// 获取用户信息
						uni.getUserInfo({
							provider: 'weixin',
							success: function(infoRes) {
								console.log('用户昵称为：' + infoRes.userInfo.nickName);
								var postData = {};
								postData.thirdapp_id = 2;

								postData.openid = loginRes.authResult.openid;
								postData.nickname = infoRes.userInfo.nickName;
								postData.headImgUrl = infoRes.userInfo.avatarUrl;
								console.log('postData', postData)
								uni.request({
									url: config.baseUrl + '/Base/ProgramToken/appLogin',
									method: 'POST',
									data: postData,
									success: function(res) {
										console.log(res)
										if (res.data && res.data.solely_code == 100000) {
											uni.setStorageSync('user_token', res.data.token);
											uni.setStorageSync('user_info', res.data.info);
											
											
												self.Router.redirectTo({
													route: {
														path: '/pages/index/index'
													}
												})
											
										} else {
											uni.showToast({
												title: '登陆失败',
												icon: 'none',
												duration: 1000,
												mask: true
											});
										};


									}
								})
								
							}
						});
					}
				});
			},



			submit() {
				const self = this;
				const postData = {
					login_name: self.submitData.login_name,
				};
				postData.smsAuth = {
					phone:self.submitData.login_name,						
					code:self.submitData.code,
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('user_token', res.token);
							uni.setStorageSync('user_info', res.info);
							setTimeout(function() {
								self.Router.redirectTo({
									route: {
										path: '/pages/index/index'
									}
								})
							}, 1000);
						} else {
							self.$Utils.showToast(res.msg, 'none')
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
	.loginBox {
		padding: 0 10%;
	}

	.items .item {
		padding: 50rpx 0 20rpx 0;
		border-bottom: 1px solid #eee;
	}

	input {
		width: 100%;
		display: block;
		box-sizing: border-box;
		font-size: 26rpx;
	}

	.wxLogin {
		position: fixed;
		bottom: 50rpx;
		left: 0;
		right: 0;
		width: 100%;
	}

	.xian {
		width: 30%;
		height: 1px;
		background: #F5F5F5;
	}

	.wxIcon {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
	}
</style>
