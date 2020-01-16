<template>
	<view>
		<view class="pdlr4 borderB1">
			
			<view>
				<textarea value="" placeholder="请输入你想说的话" v-model="submitData.content"/>
			</view>
			
			<view class="pdb15">
				<view class="flex">
					<view class="upBtn" @tap="upLoadImg('mainImg')">
						<image src="../../static/images/release-icon.png" mode=""></image>
					</view>
					<view class="mgl15 color9 fs11" v-if="submitData.mainImg.length==0">可上传多张图片</view>
				</view>
				<view class="flex" style="flex-wrap: wrap;">
					<block v-for="(image,index) in submitData.mainImg" :key="index">
						<view class="upImgLis">
							<image :src="item.url"  @tap="previewImage"></image>
						</view>
					</block>
				</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button" @click="$Utils.stopMultiClick(submit)">确定</button>
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
					name:'',
					type :1,
					content:'',
					mainImg:[],
					headImg:''
				}
			}
		},
		
		onLoad() {
			const self = this;
			uni.setStorageSync('canClick', true);
			self.submitData.name=uni.getStorageSync('user_info').info.name;
			self.submitData.headImg=uni.getStorageSync('user_info').info.mainImg;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					self.newsAdd();	
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
			
			newsAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('发送成功', 'none', 1000)
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
				self.$apis.newsAdd(postData, callback);
			},
			
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
			
			
		}
	};
</script>

<style>
	textarea{padding: 20rpx 0;}
	.upBtn{width: 120rpx;height: 120rpx;background: #F5F5F5;margin: 0 18rpx 20rpx 0;}
	.upBtn image{width: 100%;height: 100%; display: block;}
	
	.upImgLis{width: 156rpx;height: 156rpx;background: #F5F5F5;margin: 0 20rpx 20rpx 0;}
	.upImgLis:nth-of-type(4n){margin-right: 0;}
	.upImgLis image{width: 100%;height: 100%; display: block;}
</style>
