<template>
	<view>
		<view class="editLine">
			<view class="item flexRowBetween">
				<view class="ll">姓名</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">学生证</view>
				<view class="rr">
					<view class="upImg" @click="chooseImage">
						<block v-if="imageSrc">
							<image :src="imageSrc"></image>
						</block>
						<block v-else>
							<view class="addfile">
								<image src="../../static/images/real-name-icon.png" mode=""></image>
							</view>
						</block>
					</view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">学生证</view>
				<view class="rr">
					<view class="upImg" @click="chooseLicenseImage">
						<block v-if="licenseImageSrc">
							<image :src="licenseImageSrc"></image>
						</block>
						<block v-else>
							<view class="addfile">
								<image src="../../static/images/real-name-icon.png" mode=""></image>
							</view>
						</block>
					</view>
				</view>
			</view>
			<view class="fs12 color6 mglr4 pdtb15">请上传毕业证或学生证照片，已毕业用户请上传毕业证，未毕业用户请上传学生证</view>
		</view>
		
		<view class="submitbtn" style="margin-top:160rpx;">
			<button class="btn" type="button">确定</button>
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
				wx_info:{},
				imageSrc: '',
				licenseImageSrc:''
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		onUnload() {
			const self = this;
			self.imageSrc = '';
			self.licenseImageSrc = '';
		},
		methods: {
			chooseImage(){
				uni.chooseImage({
					count: 1,
					sizeType: ['compressed'],
					sourceType: ['album'],
					success: (res) => {
						console.log('chooseImage success, temp path is', res.tempFilePaths[0])
						var imageSrc = res.tempFilePaths[0]
						uni.uploadFile({
							url: 'https://unidemo.dcloud.net.cn/upload',
							filePath: imageSrc,
							fileType: 'image',
							name: 'data',
							success: (res) => {
								console.log('uploadImage success, res is:', res)
								uni.showToast({
									title: '上传成功',
									icon: 'success',
									duration: 1000
								})
								this.imageSrc = imageSrc
							},
							fail: (err) => {
								console.log('uploadImage fail', err);
								uni.showModal({
									content: err.errMsg,
									showCancel: false
								});
							}
						});
					},
					fail: (err) => {
						console.log('chooseImage fail', err)
					}
				})
			},
			chooseLicenseImage(){
				uni.chooseImage({
					count: 1,
					sizeType: ['compressed'],
					sourceType: ['album'],
					success: (res) => {
						console.log('chooseImage success, temp path is', res.tempFilePaths[0])
						var licenseImageSrc = res.tempFilePaths[0]
						uni.uploadFile({
							url: 'https://unidemo.dcloud.net.cn/upload',
							filePath: licenseImageSrc,
							fileType: 'image',
							name: 'data',
							success: (res) => {
								console.log('uploadImage success, res is:', res)
								uni.showToast({
									title: '上传成功',
									icon: 'success',
									duration: 1000
								})
								this.licenseImageSrc = licenseImageSrc
							},
							fail: (err) => {
								console.log('uploadImage fail', err);
								uni.showModal({
									content: err.errMsg,
									showCancel: false
								});
							}
						});
					},
					fail: (err) => {
						console.log('chooseImage fail', err)
					}
				})
			}
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	.editLine .item{padding: 30rpx 4%;}
	
	.upImg{width: 250rpx;height: 160rpx;background: #F5F5F5;}
	.upImg .addfile{width: 100%;height: 100%;}
	.upImg image{width: 100%;height: 100%; display: block;}
</style>
