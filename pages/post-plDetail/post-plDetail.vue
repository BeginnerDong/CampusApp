<template>
	<view>
		
		<view class="comment pdlr4 whiteBj">
			<view class="child">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-img1.png" mode=""></image></view>
					<view class="name">
						<view class="fs12">小白</view>
						<view class="fs10 color6">2019.12.25 16:31</view>
					</view>
				</view>
				<view class="font14 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了给付赛的价格肯定是个</view>
			</view>
		</view>
		
		<view class="pdlr4">
			<view class="pdt15 ftw">回复(8条)</view>
			<view class="pinglunLis">
				<view class="item" v-for="(item,index) in pinglunData" :key="index">
					<view class="flex">
						<view class="pler-photo"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name mgl10">
							<view class="fs12 color6">小白</view>
							<view class="fs10 color9">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="pdt10 fs12">监管科洛杉矶港就赶快来角色管理发送警告上了感觉监管科粉丝滤镜的规范是牢固树立感觉</view>
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
				pinglunData:[{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	page{background: #F5F5F5;}
	.comment .child{border-bottom: 0;}
	
</style>
