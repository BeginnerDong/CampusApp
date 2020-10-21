<template>
	<view>
		<!-- 评价 -->
		<view class="pdlr4 comment">
			<view class="child" v-for="(item,index) in mainData.message" :key="index" v-if="index<2">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo"><image :src="item.mainImg&&item.mainImg.length>0?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image></view>
						<view class="name">
							<view class="fs12">{{item.title}}</view>
							<view class="fs10 color6">{{item.create_time}}</view>
						</view>
					</view>
					<view class="flexEnd">
						
						<view class="starBox mgr5">
							<image v-for="(c_item,c_index) in stars" :key="c_index" :src="item.score > c_item ?(item.score/2-c_item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
							
							</image>
						</view>
					</view>
				</view>
			
				<view class="fs12 pdt10">{{item.description}}</view>
				<view class="imgbox" v-if="item.bannerImg.length>0">
					<view class="img lisThree" v-for="(c_item,c_index) in item.bannerImg" :key="c_index">
						<image :src="item.url" mode=""></image>
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
				mainData:[],
				searchItem:{
					type:4,
					user_type:0
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.relation_user = options.user_no;
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
				postData.tokenFuncName = 'getUserToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/star.css";
	@import "../../assets/style/comment.css";
	page{padding-bottom: 60rpx;}
</style>
