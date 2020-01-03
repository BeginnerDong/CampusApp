<template>
	<view>
		
		<view class="foodLis mglr4">
			<view class="item radius10 whiteBj" v-for="(item,index) in foodData" :key="index"  @click="Router.navigateTo({route:{path:'/pages/foodDetail/foodDetail'}})">
				<view class="fs15 ftw">绿茉莉 2-4人下午茶套餐</view>
				<view class="fs12 color6 pdtb5">陕西省西安市长安区韦曲街道</view>
				<view class="price fs16 ftw">128</view>
				<view class="ThreeImg flex">
					<view class="imgs"><image src="../../static/images/nip-img.png" mode=""></image></view>
					<view class="imgs"><image src="../../static/images/nip-img.png" mode=""></image></view>
					<view class="imgs"><image src="../../static/images/nip-img.png" mode=""></image></view>
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
				foodData:[{},{},{},{},{},{}]
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
	@import "../../assets/style/food.css";
	
	page{padding-bottom:60rpx;background: #F5F5F5;}
</style>
