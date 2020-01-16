<template>
	<view>
		<view class="proLis flexRowBetween mglr4 pdt15">
			<view class="item-lis boxShaow" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/integralShop-detail/integralShop-detail?id='+$event.currentTarget.dataset.id}})">
				<view class="pic">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
				</view>
				<view class="infor">
					<view class="tit avoidOverflow fs13 pdb5">{{item.title}}</view>
					<view class="red flex">
						<view class="fs11 mgr5">NIP:</view>
						<view class="ftw fs15">{{item.price}}</view>
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
				showView: false,
				wx_info:{},
				is_show:false,
				produtList:[{},{},{},{},{},{}],
				mainData:[],
				searchItem:{
					type:2,
					thirdapp_id:2,
					on_shelf:1
				},
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
			
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	/* 商品列表 */
	.proLis{flex-wrap: wrap;}
	.proLis .item-lis{ width: 330rpx; overflow: hidden;background: #fff;margin-bottom: 30rpx; border-radius: 8rpx;}
	.proLis .item-lis .pic{width: 100%;height: 300rpx;display: block;margin: 0 auto;}
	.proLis .item-lis .pic image{width: 100%; height: 100%; display: block;}
	.proLis .item-lis .infor{padding: 20rpx 4%;}
</style>
