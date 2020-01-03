<template>
	<view>
		<view class="pdlr4">
			<view class="pdtb15">内容涉及</view>
			<view class="reportLable flex fs13 center">
				<view class="btn" :class="item.check==true?'on':''" v-for="(item,index) in reportData" :key="index" @click="reportChange(index)">{{item.title}}</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top: 200rpx;">
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
				]
			}
		},
		onLoad() {
			const self = this;
		},
		methods: {
			reportChange(i){
				const self = this;
				self.reportData[i].check = !self.reportData[i].check
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
	.reportLable{flex-wrap: wrap;}
	.reportLable .btn{width: 200rpx;line-height: 60rpx;border-radius: 30rpx;background: #F5F5F5;margin: 0 44rpx 30rpx 0;}
	.reportLable .btn.on{background: #222;color: #fff;}
	.reportLable .btn:nth-of-type(3n){margin-right: 0;}
</style>
