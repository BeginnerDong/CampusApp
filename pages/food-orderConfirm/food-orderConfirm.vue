<template>
	<view>
		
		<view class="mglr4">
			<view class="coupon pdlr4 radius10 whiteBj mgt15">
				<view class="item pdtb15 flex">
					<view class="ll mgr10"><image class="pic" src="../../static/images/coupons-img.png" mode=""></image></view>
					<view class="rr fs11">
						<view class="fs14 color2 flexRowBetween">
							<view>100元代金券</view>
							<view class="color9 fs12">￥85</view>
						</view>
						<view class="color9 pdtb5">周一至周日 | 部分可用 | 免预约 | 可叠加2张</view>
						<view class="flex">
							<view class="tag flex"><image class="tagIcon" src="../../static/images/the-order-icon.png" mode=""></image><text>随时退</text></view>
							<view class="tag flex"><image class="tagIcon" src="../../static/images/the-order-icon.png" mode=""></image><text>过期自动退</text></view>
							<view class="tag flex"><image class="tagIcon" src="../../static/images/the-order-icon.png" mode=""></image><text>到店吃</text></view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="pdlr4 radius10 whiteBj mgt15">
				<view class="pdtb15 flexRowBetween borderB1">
					<view>数量</view>
					<view class="">
						<view class="numBox flex">
							<view class="btn" @click="counter('-')">-</view>
							<view class="num flexCenter">{{count}}</view>
							<view class="btn plus" @click="counter('+')">+</view>
						</view>
					</view>
				</view>
				<view class="pdtb15 flexRowBetween">
					<view>小计</view>
					<view class="price fs14">85</view>
				</view>
			</view>
			
			<view class="pdlr4 radius10 whiteBj mgt15">
				<view class="pdtb15 flexRowBetween">
					<view>手机号码</view>
					<view style="width: 50%;">
						<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween">
			<view class="left fs12 color6">合计<text class="price fs16">85</text></view>
			<view class="payBtn pubBj white"  @click="payShow">提交订单</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="payShow whiteBj radius10 center" v-show="is_payShow">
			<view class="closebtn fs18" @click="payShow">×</view>
			<view class="fs18">￥10</view>
			<view class="flexCenter pdt20">
				<view class="time fs12 color6">请在<text class="red">19:57</text>内完成支付</view>
			</view>
			<view class="submitbtn" style="margin-top: 140rpx;">
				<button class="Wbtn" type="button">立即支付</button>
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
				count:1,
				is_payShow:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			},
			payShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_payShow = !self.is_payShow;
			},
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
	@import "../../assets/style/confirm.css";
	
	page{padding-bottom:150rpx;background: #F5F5F5;}
	input{width: 100%; line-height: 48rpx;display: block;box-sizing: border-box;font-size: 26rpx; color: #666;text-align: right;}
</style>
