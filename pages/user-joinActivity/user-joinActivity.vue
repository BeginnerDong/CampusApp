<template>
	<view>
		<view class="joinActive mglr4">
			<view class="item" v-for="(item,index) in mainData" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="pdb10">{{item.title}}</view>
				<view class="quanCard fs15">
					<view class="center fs22 ftw quanTit pdb15">入场券</view>
					<view>时间：{{item.start_time}}</view>
					<view class="pdt10">地点：{{item.address}}</view>
				</view>
				<view class="flexRowBetween pdt15 fs13">
					<view>发布者：</view>
					<view class="flexEnd">
						<view><image class="photo" :src="item.headImg&&item.headImg[0]?item.headImg[0].url:''" mode=""></image></view>
						<view class="mgl10">{{item.name}}：{{item.phone}}</view>
					</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				searchItem:{
					type:2,
					report:0,
					user_type:0
				},
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			console.log(2323)
			self.$Utils.loadAll(['getMainData'], self);
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
			
			getMainData(isNew) {
				const self = this;
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
				postData.getBefore = {
					test: {
						tableName: 'Log',
						searchItem: {
							relation_table: ['in', ['News']],
							type:['in',[7]],
							user_no:['in',[uni.getStorageSync('user_info').user_no]]
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].start_time = self.$Utils.timeto(self.mainData[i].start_time*1000,'ymd')
						}
					}
			
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.newsGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	.joinActive .item{padding: 30rpx;background: #fff;border-radius: 10rpx;margin-top: 30rpx;position: relative;}
	.quanCard{width: 100%;height: 316rpx;padding: 50rpx 30rpx 30rpx 210rpx;background: url(../../static/images/canyuhuodong-img.png) no-repeat 0 0/100% 100%;box-sizing: border-box;}
	.joinActive .item .photo{width: 70rpx;height: 70rpx;border-radius: 50%;}
	.overdue{width: 100rpx; height: 100rpx;position: absolute;top: 0;right: 0;}
	.overdue image{width: 100%;height: 100%; display: block}
</style>
