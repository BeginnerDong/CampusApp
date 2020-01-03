<template>
	<view>
		
		<view class="myRowBetween pdlr4">
			<view class="item flexRowBetween" v-for="(item,index) in followData" :key="index" >
				<view class="ll flex" @click="Router.navigateTo({route:{path:'/pages/TA-Home/TA-Home'}})">
					<view class="photo"><image src="../../static/images/xiaoxi-img.png" mode=""></image></view>
					<view class="ll-tit avoidOverflow">世界摄影大师</view>
				</view>
				<view class="rr">
					<view class="joinOK flexCenter fs12"  @click="alertBoxShow">
						<image class="gzIcon" src="../../static/images/jia.png" mode=""></image>
						<text>关注</text>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 加入提示弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="alertBox center pdt20 radius10 whiteBj" v-show="is_alertBox">
			<view>提示：</view>
			<view class="pdt10 pdb20 color6 fs13">确定不再关注？</view>
			<view class="flexRowBetween selt seltBtn fs13">
				<view class="half" @click="alertBoxShow">取消</view>
				<view class="half">确定</view>
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
				is_show:false,
				is_alertBox:false,
				followData:[{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			alertBoxShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_alertBox = !self.is_alertBox
			}
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/alertBox.css";
	
	.myRowBetween .item .ll{width: 70%;}
	.myRowBetween .item .rr{width: 30%;}
	.joinOK{padding: 0 10rpx;width: 140rpx;text-align: center;line-height: 50rpx;border-radius: 30rpx;border: 1px solid #eee;box-sizing: border-box;}
	.joinOK .gzIcon{width: 22rpx;height: 22rpx;display: block;margin-right: 10rpx;}
</style>
