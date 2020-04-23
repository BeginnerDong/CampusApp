<template>
	<view>
		<view class="editLine">
			<view class="item flexRowBetween">
				<view class="ll">姓名</view>
				<view class="rr">
					<input type="text" v-model="submitData.real_name" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">学生证</view>
				<view class="rr">
					<view class="upImg" @click="upLoadImg('academicImg')">
						<block v-if="submitData.academicImg.length>0">
							<image :src="submitData.academicImg&&submitData.academicImg[0]?submitData.academicImg[0].url:''"></image>
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
				<view class="ll">毕业证</view>
				<view class="rr">
					<view class="upImg" @click="upLoadImg('graduationImg')">
						<block v-if="submitData.graduationImg.length>0">
							<image :src="submitData.graduationImg&&submitData.graduationImg[0]?submitData.graduationImg[0].url:''"></image>
						</block>
						<block v-else>
							<view class="addfile">
								<image src="../../static/images/real-name-icon.png" mode=""></image>
							</view>
						</block>
					</view>
				</view>
			</view>
			<view class="fs12 color6 mglr4 pdtb15">请上传毕业证或学生证照片，已毕业用户请上传毕业证或者身份证，未毕业用户请上传学生证</view>
		</view>
		
		<view class="submitbtn" style="margin-top:160rpx;">
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
					real_name:'',	
					academicImg:[],
					graduationImg:[],
					check_status:1
				}
			}
		},
		onLoad() {
			const self = this;
			uni.setStorageSync('canClick', true);
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		onUnload() {
			const self = this;
	
		},
		
		methods: {
			
			upLoadImg(type) {
				const self = this;			
				uni.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
						uni.hideLoading();
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getUserToken',
							
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.real_name==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入真实姓名', 'none', 1000)
				}else{
					self.userInfoUpdate()
				}
			},
			
			userInfoUpdate() {
				const self = this;
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
						self.$Utils.showToast('认证成功，等待后台审核', 'none', 1000)
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
				self.$apis.userInfoUpdate(postData, callback);
			},
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
