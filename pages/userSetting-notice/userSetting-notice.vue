<template>
	<view>
		
		<view class="dialogBox pdlr4 f5bj">
			<view class="item left" v-for="item in mainData">
				<view class="time center fs13 color9">{{item.create_time}}</view>
				<view class="infor">
					<image class="Photo" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:'../../static/images/about-img.png'" >
					<view class="text">{{item.content}}</view>
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
				score:'',
				wx_info:{},
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
					self.systemData = [];
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
					type:1,
					user_no:uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/tidings.css";
	page{background: #F5F5F5;}
	
</style>
