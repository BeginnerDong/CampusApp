<template>
	<view>
		
		<view class="myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">昵称</view>
				<view class="rr">
					<input type="text" maxlength="11" v-model="submitData.name" placeholder="昵称">
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">性别</view>
				<view class="rr">
					<picker mode="selector" :range="genderData" @change="change" data-key="gender">
						<view>{{submitData.gender==1?'男':(submitData.gender==2?'女':'性别')}}</view>
					</picker>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">学校</view>
				<view class="rr">
					<input type="text" v-model="submitData.school" placeholder="学校">
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">学历</view>
				<view class="rr">
					<picker mode="date"  @change="timeChange">
						<view>{{submitData.admission_time!=''?submitData.admission_time:'入学时间'}}</view>
					</picker>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">入学时间</view>
				<view class="rr">
					<picker mode="selector" :range="educationData" @change="change" data-key="education">
						<view>{{educationData[educationIndex]?educationData[educationIndex]:'学历'}}</view>
					</picker>
				</view>
			</view>
		</view>
		
		<view class="mglr4 pdtb15 flex">
			<view class="gzBtn fs12 white center" style="width: 140rpx;background: #666;"  
			@click="Router.navigateTo({route:{path:'/pages/user-realname/user-realname'}})">实名认证</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 150rpx;">
			<button class="btn" type="button" @click="Utils.stopMultiClick(submit)">保存</button>
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
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			prev(){
				const self = this;
				self.$Router.back(1)
			},
			
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
						self.submitData.school = self.mainData.school;
						self.submitData.education = self.mainData.education;
						for (var i = 0; i < self.educationData.length; i++) {
							if(self.educationData[i]==self.mainData.education){
								self.educationIndex = i
							}
						}
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
	.myRowBetween .item{padding: 30rpx 4%;}
</style>
