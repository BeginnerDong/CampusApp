<template>
	<view>
		
		<view class="pdlr4 whiteBj">
			<view class="flexRowBetween pdtb10">
				<view @click="prev()"><image class="backIcon" style="margin-left: 0;" src="../../static/images/arrowL2.png" mode=""></image></view>
				<view class="gzBtn fs12 center white" style="background: #666;" @click="$Utils.stopMultiClick(userInfoUpdate)">确定</view>
			</view>
		</view>
		<view class="pdlr4 mgt15">
			<view class="pdb10" style="border-bottom: 1px solid #ddd;">
				<input type="text" v-model="submitData.role" placeholder="请输入角色" placeholder-class="placeholder">
			</view>
			<view class="pdtb10 fs12 color9">更加准备的让别人了解你。</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				is_show:false,
				submitData:{
					role:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.submitData.role = options.name
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			prev(){
				const self = this;
				self.$Router.back(1)
			},
			
			userInfoUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.role==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('角色不能为空', 'none', 1000)
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						uni.navigateBack({
							delta:1
						})
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/userHome.css";
	page{background: #F5F5F5;}
</style>
