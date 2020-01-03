<template>
	<view>
		
		<view class="questionCont">
			<view class="item"  v-for="(item,inedx) in questionData" :key="index">
				<view class="line flex">
					<view><image class="icon" src="../../static/images/help-icon.png" mode=""></image></view>
					<view class="Rtext fs13">问题标题问题标签房贷计算开个会？</view>
				</view>
				<view class="line flex mgt5">
					<view><image class="icon" src="../../static/images/help-icon1.png" mode=""></image></view>
					<view class="Rtext fs12 color6">个梵蒂冈肉上面考虑过可是仍然就孤苦伶仃就是个回访近凯撒大帝老汉给华工科技放声大哭和公司公司换个飞机肯定是个</view>
				</view>
			</view>
		</view>
		
		<view class="FX-Kefu whiteBj flexCenter boxShaow">
			<view class="cont flexRowBetween radius10 boxShaow">
				<view class="fs12">没找到问题，咨询客服</view>
				<view><image class="kfIcon" src="../../static/images/help-icon2.png" mode=""></image></view>
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
				questionData:[{},{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		}
	};
</script>

<style>
	/* @import "../../assets/style/tidings.css"; */
	page{padding-bottom: 160rpx;}
	.questionCont .item{padding: 30rpx 4%;border-bottom: 10rpx solid #F5F5F5;}
	.questionCont .item .icon{width: 28rpx;height: 28rpx; display: block;margin:6rpx 20rpx 0 0 ;}
	.questionCont .line{align-items: flex-start;}
	
	.FX-Kefu{padding: 20rpx 4%;height: 160rpx;position: fixed;left: 0; right: 0;bottom: 0rpx;background: #FFF;box-sizing: border-box;}
	.FX-Kefu .cont{padding: 20rpx 4%;width: 100%;box-sizing: border-box;}
	.kfIcon{width: 50rpx;height: 50rpx; display: block;}
</style>
