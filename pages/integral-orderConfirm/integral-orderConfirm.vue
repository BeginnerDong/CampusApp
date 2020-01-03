<template>
	<view>
		
		<view class="mglr4">
			<view class="coupon pdlr4 radius10 whiteBj mgt15">
				<view class="item pdtb15 flex">
					<view class="ll mgr10"><image class="pic" src="../../static/images/coupons-img.png" mode=""></image></view>
					<view class="rr fs11 pr">
						<view class="fs14 color2 pdt5 avoidOverflow2">双菇蒸饺</view>
						<view class="red fs12 B-price">NIP:<text class="mgl5 ftw fs15">56.00</text></view>
					</view>
				</view>
			</view>
			
			
			
			<view class="pdlr4 radius10 whiteBj mgt15">
				<view class="pdtb15 flexRowBetween">
					<view>购买数量</view>
					<view class="">
						<view class="numBox flex">
							<view class="btn" @click="counter('-')">-</view>
							<view class="num flexCenter">{{count}}</view>
							<view class="btn plus" @click="counter('+')">+</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="radius10 whiteBj mgt15 editLine">
				<view class="item flexRowBetween">
					<view class="ll">姓名：</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入姓名" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">电话：</view>
					<view class="rr">
						<input type="number" value="" placeholder="请输入联系方式" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">地址：</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入地址" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexEnd">
			<view class="left fs12 color6 mgr15">合计：<text class="red fs15">36.00</text></view>
			<view class="payBtn pubBj white"  @click="payShow">立即兑换</view>
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
	@import "../../assets/style/editInfor.css";
	
	page{padding-bottom:150rpx;background: #F5F5F5;}
	
	.coupon .pic{width: 180rpx;height: 180rpx;}
	.coupon .rr{width: 68%;height: 180rpx;box-sizing: border-box;}
	.B-price{position: absolute;left: 0;bottom: 10rpx;right: 0;}
	
	.editLine .item{padding: 30rpx 4%;}
	
	.xqbotomBar{padding: 0;}
	.xqbotomBar .payBtn{border-radius: 0;line-height: 100rpx;}
</style>
