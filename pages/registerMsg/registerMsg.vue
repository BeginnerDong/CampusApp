<template>
	<view>
		
		<view class="registerBox">
			<view class="items fs13 color6">
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon.png" mode=""></image></view>
					<view class="rr">
						<input type="text" maxlength="11" v-model="submitData.name" placeholder="昵称">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon1.png" mode=""></image></view>
					<view class="rr">
						<picker mode="selector" :range="genderData" @change="change" data-key="gender">
							<view>{{submitData.gender==1?'男':(submitData.gender==2?'女':'性别')}}</view>
						</picker>
						<!-- <input type="text" value="" placeholder="性别"> -->
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon2.png" mode=""></image></view>
					<view class="rr">
						<input type="text" v-model="submitData.school" placeholder="学校">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon3.png" mode=""></image></view>
					<view class="rr">
						<picker mode="date"  @change="timeChange">
							<view>{{submitData.admission_time!=''?submitData.admission_time:'入学时间'}}</view>
						</picker>
						<!-- <input type="number" maxlength="11" value="" placeholder="入学时间"> -->
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/perfect-information-icon4.png" mode=""></image></view>
					<view class="rr">
						<picker mode="selector" :range="educationData" @change="change" data-key="education">
							<view>{{educationData[educationIndex]?educationData[educationIndex]:'学历'}}</view>
						</picker>
						<!-- <input type="number" maxlength="11" value="" placeholder="学历"> -->
					</view>
				</view>
			</view>
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="Wbtn" type="button" @click="Utils.stopMultiClick(submit)">提交</button>
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
					name:'',
					gender:'',
					school:'',
					education:'',
					admission_time:''
				},
				genderData:['男','女'],
				genderIndex:-1,

				educationData:['高中及以下','大专','本科','研究生及以上'],
				educationIndex:-1
			}
		},
		onLoad() {
			const self = this;
		},
		methods: {
			
			timeChange(e){
				const self = this;
				self.submitData.admission_time = e.detail.value
			},
			
			change(e){
				const self = this;
				console.log(e)
				self[e.currentTarget.dataset.key+'Index'] = e.detail.value;
				self.submitData[e.currentTarget.dataset.key] = self[e.currentTarget.dataset.key+'Data'][e.detail.value];
				console.log(self.submitData);
				if(self.submitData.gender=='男'){
					self.submitData.gender = 1
				}else if(self.submitData.gender=='女'){
					self.submitData.gender = 2
				}
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.name = self.mainData.name;
						self.submitData.gender = self.mainData.gender;
						self.submitData.admission_time = self.mainData.admission_time;
						//self.birth = self.$Utils.timeto(self.submitData.birth*1000,'ymd')
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					self.userInfoUpdate();	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
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
						self.$Utils.showToast('保存成功', 'none');
						setTimeout(function() {
							self.Router.redirectTo({route:{path:'/pages/index/index'}})
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
	
	.registerBox{padding: 0 10%;}
	.items .item{padding: 50rpx 0 20rpx 0;border-bottom: 1px solid #eee; align-items: flex-start;}
	.items .item .icon{padding: 0 30rpx;width: 15%;box-sizing: border-box;}
	.items .item .icon image{width: 32rpx;height: 32rpx;display: block;}
	input{width: 100%; display: block;box-sizing: border-box;font-size: 26rpx;}
	.items .item .rr{width: 85%;}
	
</style>
