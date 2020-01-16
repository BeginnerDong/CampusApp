<template>
	<view>
		<view class="myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">社区图像</view>
				<view class="rr">
					<view class="Rphoto mgr10" @click="upLoadImg('mainImg')">
						<image :src="submitData.mainImg&&submitData.mainImg[0]?submitData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
					</view>
					<view><image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">社区名称</view>
				<view class="rr">
					<input type="text" v-model="submitData.title" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item">
				<view>社区简介</view>
				<view class="pdt15">
					<textarea v-model="submitData.description" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top:160rpx;">
			<button class="btn" type="button" @click="$Utils.stopMultiClick(submit)">提交</button>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				submitData:{
					title:'',
					mainImg:[],
					description:'',
				}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					self.communityAdd();	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			upLoadImg(type) {
				const self = this;	
				if (self.submitData[type].length > 8) {
					api.showToast('仅限9张', 'fail');
					return;
				};
				wx.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] =[];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log('type',type)
						console.log(self.submitData)
						wx.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 9,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths;
						console.log(callback)
						for (var i = 0; i < tempFilePaths.length; i++) {
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								tokenFuncName: 'getUserToken'
							}, callback)
						}
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
			communityAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.addLog(data.info.id)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.communityAdd(postData, callback);
			},
			
			addLog(id) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				
				postData.data = {};
				postData.data = {
					type:6,
					relation_table:'Community',
					relation_id:id,
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('创建成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.logAdd(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	.myRowBetween .item{padding: 30rpx 4%;}
	
	.Rphoto{width: 80rpx;height: 80rpx;background: #fff;padding: 4rpx;border-radius: 50%;border: 1px solid #eee; overflow: hidden;}
	.Rphoto image{width: 100%;height: 100%; display: block;}
	
	.myRowBetween .item textarea{height: 240rpx;background: #F5F5F5;border-radius: 10rpx;border: 0;}
</style>
