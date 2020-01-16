<template>
	<view>
		<view class="positionList pdlr4">
			<view class="item flexRowBetween pdtb15" v-for="item in mainData" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/positionDetail/positionDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="flex ll">
					<view>
						<image class="Lphoto" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
					</view>
					<view class="infor">
						<view class="name">{{item.title}}</view>
						<view class="lable fs13 color6">{{item.company}}</view>
						<view class="fs12 color9">{{item.city}}&nbsp;|&nbsp;{{item.experience}}&nbsp;|&nbsp;{{item.education}}</view>
					</view>
				</view>
				<view class="rr red fs13">{{item.salary}}</view>
			</view>
		</view>	
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
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
							title: ['in', ['加入我们']],
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
	page{padding-bottom: 60rpx;}
	
	.positionList .item{border-bottom: 1px solid #F5F5F5;align-items: flex-start;}
	.positionList .item .ll{width: 80%;}
	.positionList .item .fxIcon{width: 30px; height: 38px;position: absolute; top: 0;left: 0;}
	.positionList .item .fxIcon img{width: 100%; height: 100%; display: block;}
	.positionList .item .rr{width: 20%;text-align: right;}	
	.positionList .item .ll .Lphoto{width: 55px;height: 55px;display: block;margin-right: 10px;}
	.positionList .item .ll .infor{width: 70%;}
	.positionList .item .ll .infor .lable{color: #666;line-height: 16px;padding: 3px 0;}
</style>
