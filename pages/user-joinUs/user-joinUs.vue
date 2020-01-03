<template>
	<view>
		<view class="positionList pdlr4">
			<view class="item flexRowBetween pdtb15" v-for="item in positionDate" @click="Router.navigateTo({route:{path:'/pages/positionDetail/positionDetail'}})">
				<view class="flex ll">
					<view>
						<image class="Lphoto" src="../../static/images/join-us-img.png"></image>
					</view>
					<view class="infor">
						<view class="name">会计助理</view>
						<view class="lable fs13 color6">克莱</view>
						<view class="fs12 color9">西安&nbsp;|&nbsp;3年&nbsp;|&nbsp;本科</view>
					</view>
				</view>
				<view class="rr red fs13">4K-6K</view>
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
				positionDate:[{},{},{}]
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
