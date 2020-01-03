<template>
	<view>
		
		<view class="mglr4">
			<view class="CouponCard">
				<view class="item">
					<view class="fs10">绿茶莉餐厅代金券</view>
					<view class="fs16 pdt5 pdb10 ftw">85元代100元券</view>
					<view class="xian"></view>
					<view class="flexRowBetween pdt10 fs11">
						<view>周一至周天可用</view>
						<view class="fs10 color9">10:00-22:00</view>
					</view>
				</view>
			</view>
			
			<view class="pdlr4 pdt15 pdb15 radius10 whiteBj mgt15">
				<view class="">使用规则</view>
				<view class="xqInfor fs12">
					<view>单次可用2张 随时退 免预约 过期自动退</view>
					<view>有效期：2019.12.30-2019.12.31 23:59 (周末、法定节假日通用)</view>
					<view>使用时间：11:30-22:00,南门店:029-23562356</view>
					<view>适用范围：除酒水外全场通用</view>
					<view>除酒水饮料外全场通用</view>
					<view>无需预约，消费高峰期可能需要等位</view>
				</view>
				<view class="pdt10 borderB1"></view>
				<view class="flexEnd fs12 pdt10">更多规则<image class="arrowB" src="../../static/images/coupons-icon1.png" mode=""></image></view>
			</view>
			
			<view class="pdlr4 pdt15 pdb15 radius10 whiteBj mgt15 canTing">
				<view class="pdb10">餐厅介绍</view>
				<view class="flex" @click="Router.redirectTo({route:{path:'/pages/shopDetail/shopDetail'}})">
					<view class="pic">
						<image src="../../static/images/coupons-img.png" mode=""></image>
					</view>
					<view class="infor mgl10  fs12 color6" style="width: 70%;">
						<view class="fs14 color2 ftw">绿茉莉</view>
						<view class="flex" style="flex-wrap: wrap;">
							<view class="lable">甜点</view>
						</view>
						<view class="adrs mgt15">南稍门</view>
					</view>
				</view>
			</view>
			
			<!-- 底部菜单按钮 -->
			<view class="xqbotomBar flexRowBetween">
				<view class="left price fs16">85</view>
				<view class="payBtn pubBj white" @click="Router.navigateTo({route:{path:'/pages/food-orderConfirm/food-orderConfirm'}})">立即抢购</view>
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
	@import "../../assets/style/foodDetail.css";
	
	page{padding-bottom:150rpx;background: #F5F5F5;}
	.xqInfor view{margin-top: 10rpx;}
	
</style>
