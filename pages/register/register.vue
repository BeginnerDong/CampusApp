<template>
	<view>
		<view class="loginBox">
			<view class="fs18 ftw pdt30 pdb10">注册账号</view>
			<view class="items fs13 color6">
				<view class="item flex borderB1">
					<input type="number" maxlength="11" v-model="submitData.phone" placeholder="手机号">
				</view>
				<view class="item flex borderB1 flexRowBetween">
					<view style="width: 50%;">
						<input type="text"  value="" placeholder="验证码" v-model="submitData.code">
					</view>
					<view class="fs14 ftw color2">获取验证码</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 240rpx;">
				<button class="Wbtn" type="button" @click="Utils.stopMultiClick(submit)">确定</button>
			</view>
			
		</view>
		
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					phone:'',
					code:''
				}
			}
		},
		
		onLoad() {
			const self = this;
			uni.setStorageSync('canClick',true)
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick',false)
				const postData = {
					data:self.$Utils.cloneForm(self.submitData)					
				}
				/* postData.smsAuth = {						
					phone:self.submitData.phone,						
					code:self.submitData.smsCode,
				}; */
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.code;
				if (self.$Utils.checkComplete(newObject)) {						
					const callback = (res) => {
						uni.setStorageSync('canClick',true)
						if (res.solely_code == 100000) {
							self.$Utils.showToast(res.msg, 'none');	
							uni.setStorageSync('user_token', res.token);
							uni.setStorageSync('user_info', res.info);
							setTimeout(function() {
								self.Router.redirectTo({route:{path:'/pages/registerMsg/registerMsg'}})
							}, 1000);
						} else {
							self.$Utils.showToast(res.msg, 'none');
						}
					}
					self.$apis.registerUser(postData, callback);
				} else {
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请输入手机号', 'none');
				};
			},
		}
	};
</script>

<style>
	.loginBox{padding: 0 10%;}
	.items .item{padding: 50rpx 0 20rpx 0;border-bottom: 1px solid #eee;}
	input{width: 100%; display: block;box-sizing: border-box;font-size: 26rpx;}
</style>
