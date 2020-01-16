<template>
	<view>
		<view class="pdlr4">
			<view class="pdtb15">内容涉及</view>
			<view class="reportLable flex fs13 center">
				<view class="btn" :class="item.check==true?'on':''" v-for="(item,index) in reportData" :key="index" @click="reportChange(index)">{{item.title}}</view>
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
				showView: false,
				wx_info:{},
				is_show:false,
				reportData:[
					{check:false,title:'涉及赌博'},
					{check:false,title:'公司买卖'},
					{check:false,title:'代理记账'},
					{check:false,title:'资质代办'},
					{check:false,title:'商标过户'},
					{check:false,title:'专利过户'},
					{check:false,title:'挂靠地址'},
					{check:false,title:'执照加快'},
				],
				submitData:{
					type:8,
					title:'',
					relation_id:'',
					relation_table:'News',
					
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.submitData.relation_id = options.id;
		},
		
		methods: {
			
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					self.logAdd();	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			
			
			logAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('举报成功，等待后台审核', 'none', 1000)
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
			
			reportChange(i){
				const self = this;
				self.submitData.title='';
				self.reportData[i].check = !self.reportData[i].check;
				for (var i = 0; i < self.reportData.length; i++) {
					if(self.reportData[i].check){
						self.submitData.title = self.submitData.title + self.reportData[i].title+','
					}
				}
				console.log('self.submitData',self.submitData)
			},

		}
	};
</script>

<style>
	.reportLable{flex-wrap: wrap;}
	.reportLable .btn{width: 200rpx;line-height: 60rpx;border-radius: 30rpx;background: #F5F5F5;margin: 0 44rpx 30rpx 0;}
	.reportLable .btn.on{background: #222;color: #fff;}
	.reportLable .btn:nth-of-type(3n){margin-right: 0;}
</style>
