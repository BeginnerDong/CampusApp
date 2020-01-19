<template>
	<view>
		
		<view class="questionCont">
			<view class="item"  v-for="(item,inedx) in mainData" :key="index">
				<view class="line flex">
					<view><image class="icon" src="../../static/images/help-icon.png" mode=""></image></view>
					<view class="Rtext fs13">{{item.title}}</view>
				</view>
				<view class="line flex mgt5">
					<view><image class="icon" src="../../static/images/help-icon1.png" mode=""></image></view>
					<view class="Rtext fs12 color6">{{item.description}}</view>
				</view>
			</view>
		</view>
		
		<view class="FX-Kefu whiteBj flexCenter boxShaow" @click="phoneCall()">
			<view class="cont flexRowBetween radius10 boxShaow">
				<view class="fs12">没找到问题，咨询客服</view>
				<view><image class="kfIcon" src="../../static/images/help-icon2.png" mode=""></image></view>
			</view>
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
				questionData:[{},{},{},{},{},{}],
				mainData:[],
				labelData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			console.log(2323)
			self.$Utils.loadAll(['getMainData','getLabelData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getLabelData() {
				const self = this;
				console.log(2323)
				const postData = {};
				postData.searchItem = {
					title:'帮助与客服'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0]
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			phoneCall() {
				const self = this;
				wx.makePhoneCall({
					phoneNumber: self.labelData.description,
				})
			},
			
			getMainData(isNew) {
				const self = this;
				console.log(2323)
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['帮助与客服']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	/* @import "../../assets/style/tidings.css"; */
	page{padding-bottom: 160rpx;}
	.questionCont .item{padding: 30rpx 4%;border-bottom: 10rpx solid #F5F5F5;}
	.questionCont .item .icon{width: 28rpx;height: 28rpx; display: block;margin:6rpx 20rpx 0 0 ;}
	.questionCont .line{align-items: flex-start;}
	
	.FX-Kefu{padding: 20rpx 4%;height: 160rpx;position: fixed;left: 0; right: 0;bottom: 0rpx;background: #FFF;box-sizing: border-box;}
	.FX-Kefu .cont{padding: 20rpx 4%;width: 100%;box-sizing: border-box;}
	.kfIcon{width: 50rpx;height: 50rpx; display: block;}
</style>
