<template>
	<view>
		
		<view class="comment pdlr4 whiteBj">
			<view class="child">
				<view class="flex">
					<view class="photo">
						<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="name">
						<view class="fs12">{{mainData.title}}</view>
						<view class="fs10 color6">{{mainData.create_time}}</view>
					</view>
				</view>
				<view class="font14 pdt10">
					{{mainData.content}}
				</view>
			</view>
		</view>
		
		<view class="pdlr4">
			<view class="pdt15 ftw">回复({{mainData.reply.length}}条)</view>
			<view class="pinglunLis">
				<view class="item" v-for="(item,index) in mainData.reply" :key="index">
					<view class="flex">
						<view class="pler-photo">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="name mgl10">
							<view class="fs12">{{item.title}}</view>
							<view class="fs10 color6">{{item.create_time}}</view>
						</view>
					</view>
					<view class="pdt10 fs12">
						{{item.content}}
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
			
				pinglunData:[{},{},{},{},{}],
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					thirdapp_id:2,
					user_type:0,
					id:self.id
				};
				postData.getAfter = {
					reply: {
						tableName: 'Message',
						searchItem: {
							status:1,
							type:2,
							user_type:0,
						},
						middleKey: 'id',
						key: 'passage1',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	page{background: #F5F5F5;}
	.comment .child{border-bottom: 0;}
	
</style>
