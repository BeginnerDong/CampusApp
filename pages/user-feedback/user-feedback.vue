<template>
	<view>
		<view class="pdtb15 borderB1 mglr4">
			<textarea v-model="submitData.content" placeholder="请输入您对此平台的意见" placeholder-class="placeholder" />
		</view>
		
		<view class="submitbtn" style="margin-top:200rpx;">
			<button class="btn" type="button"  @click="Utils.stopMultiClick(addMessage)">提交</button>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				submitData:{
					type:3,
					content:'',
					title:'',
					mainImg:[]
				}
			}
		},
		
		onLoad() {
			const self = this;
			uni.setStorageSync('canClick', true);
			elf.submitData.title=uni.getStorageSync('user_info').info.name;
			self.submitData.mainImg=uni.getStorageSync('user_info').info.mainImg;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			addMessage() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.content==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('评论不能为空', 'none', 1000);
					return
				};
				const postData = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.$Utils.showToast('反馈成功', 'none', 1000);
						uni.navigateBack({
							delta:1
						})
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.messageAdd(postData, callback);
			},
		}
	};
</script>

<style>
	page{padding-bottom: 60rpx;}
	
	textarea{height: 600rpx;border: 0;padding: 0;}
</style>
