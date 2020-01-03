<template>
	<view>
		
		<view class="comment mglr4 mgt15">
			<view class="child pr radius10 oh">
				<image class="FX-icon" src="../../static/images/activity-icon1.png" mode=""></image>
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="gzBtn fs12 center white">关注</view>
					</view>
				</view>
				<view class="ftw pdt10 pdb5">参观大唐芙蓉园</view>
				<view class="fs12 color6">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了规划设计法海跟is干活换个飞机多少个扩大复合弓鹤骨鸡肤大好时光</view>
				<view class="imgbox">
					<view class="img lisThree">
						<image src="../../static/images/the-order-img.png" mode=""></image>
					</view>
					<view class="img lisThree">
						<image src="../../static/images/the-order-img.png" mode=""></image>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs12">
					<view class="lis flex">
						<image src="../../static/images/activity-icon2.png" mode=""></image>
						<view>收藏</view>
					</view>
					<view class="lis flex">
						<image src="../../static/images/home-icon5.png" mode=""></image>
						<view>1562</view>
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
				is_show:false
			}
		},
		
		onLoad(options) {
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
	page{padding-bottom: 60rpx;}
	
</style>

