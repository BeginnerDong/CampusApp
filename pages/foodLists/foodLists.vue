<template>
	<view>
		
		<view class="foodLis mglr4">
			<view class="item radius10 whiteBj" v-for="(item,index) in mainData" :key="index"  :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/food-orderConfirm/food-orderConfirm?id='+$event.currentTarget.dataset.id}})">
				<view class="fs15 ftw">{{item.title}}</view>
				<view class="fs12 color6 pdtb5">{{item.shop?item.shop.address:''}}</view>
				<view class="price fs16 ftw">{{item.price}}</view>
				<view class="ThreeImg flex">
					<view class="imgs" v-for="c_item in item.mainImg"><image :src="c_item.url" mode=""></image></view>
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
				foodData:[{},{},{},{},{},{}],
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					type:1,
					on_shelf:1
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.name = options.name;
			
			uni.setNavigationBarTitle({
			    title: self.name
			});
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
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['in', [self.name]],
						},
						middleKey: 'category_id',
						key: 'id',
						condition: 'in',
					},
				};
				postData.getAfter = {
					shop:{
						token:uni.getStorageSync('user_token'),
						tableName:'UserInfo',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['address']
					}
				};
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
	@import "../../assets/style/food.css";
	
	page{padding-bottom:60rpx;background: #F5F5F5;}
</style>
