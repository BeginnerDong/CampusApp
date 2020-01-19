<template>
	<view>
		
		<view class="myRowBetween pdlr4">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index"  :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/user-mySheQu-Home/user-mySheQu-Home?id='+$event.currentTarget.dataset.id}})">
				<view class="ll flex">
					<view class="photo">
						<image :src="item.mainImg&&item.mainImg.length>0?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
					</view>
					<view class="ll-tit avoidOverflow">{{item.title}}</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image>
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
					thirdapp_id:2,
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
							relation_table: ['in', ['Community']],
							type:['in',[6]],
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
				self.$apis.communityGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/alertBox.css";
	
	.myRowBetween .item .ll{width: 70%;}
	.myRowBetween .item .rr{width: 30%;}
	.joinOK{padding: 0 10rpx;width: 140rpx;text-align: center;line-height: 50rpx;border-radius: 30rpx;border: 1px solid #eee;box-sizing: border-box;}
	.joinOK .okIcon{width: 21rpx;height: 15rpx;display: block;margin-right: 10rpx;}
</style>
