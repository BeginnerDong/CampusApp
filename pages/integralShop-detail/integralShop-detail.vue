<template>
	<view>
		
		<view class="detailBaner">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		<view class="mglr4 pdtb10">
			<view class="fs14 pdb5" >{{mainData.title}}</view>
			<view class="red fs15 ftw"><text class="ftn fs12 mgr5">NIP:</text>{{mainData.price}}</view>
		</view>	
		<view class="f5H5"></view>
		
		<view class="mglr4">
			<view class="pdt15 pdb10">商品详情</view>
			<view class="xqInfor">
				<view class="content ql-editor"  v-html="mainData.content">
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 60rpx">
			<button class="btn" type="button"  v-if="userInfoData.check_status==2"
			 @click="Router.navigateTo({route:{path:'/pages/integral-orderConfirm/integral-orderConfirm?id='+mainData.id}})">兑换</button>
			 <button class="btn" type="button"  v-if="userInfoData.check_status!=2" @click="realnameshow">兑换</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				userInfoData:{},
				is_show:false,
				is_realnameshow:false
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		methods: {
			
			realnameshow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_realnameshow = !self.is_realnameshow
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				 const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	page{padding-bottom: 60rpx;}
	.detailBaner image{width: 100%;height: 360rpx; display: block;}
	.xqInfor view {font-size: 26rpx;color: #666;padding-bottom: 20rpx;}
	.xqInfor image{max-width: 100% !important; display: block;margin: 20rpx auto;}
	.realnameshow{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 42;}
	.realnameshow .bjImg{width: 100%;display: block;}
</style>
