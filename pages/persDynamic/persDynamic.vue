<template>
	<view>
		<view class="pdlr4 borderB1">
			
			<view>
				<textarea value="" placeholder="请输入你想说的话" />
			</view>
			
			<view class="pdb15">
				<view class="flex">
					<view class="upBtn" @tap="chooseImage"><image src="../../static/images/release-icon.png" mode=""></image></view>
					<view class="mgl15 color9 fs11">可上传多张图片</view>
				</view>
				<view class="flex" style="flex-wrap: wrap;">
					<block v-for="(image,index) in imageList" :key="index">
						<view class="upImgLis">
							<image :src="image" :data-src="image" @tap="previewImage"></image>
						</view>
					</block>
				</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/index/index'}})">确定</button>
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
				is_show:false,
				imageList: []
			}
		},
		onUnload() {
			this.imageList = []
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			chooseImage: async function() {
				uni.chooseImage({
					success: (res) => {
						this.imageList = this.imageList.concat(res.tempFilePaths);
					}
				})
			},
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
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
	textarea{padding: 20rpx 0;}
	.upBtn{width: 120rpx;height: 120rpx;background: #F5F5F5;margin: 0 18rpx 20rpx 0;}
	.upBtn image{width: 100%;height: 100%; display: block;}
	
	.upImgLis{width: 156rpx;height: 156rpx;background: #F5F5F5;margin: 0 20rpx 20rpx 0;}
	.upImgLis:nth-of-type(4n){margin-right: 0;}
	.upImgLis image{width: 100%;height: 100%; display: block;}
</style>
