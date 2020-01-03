<template>
	<view>
		
		<view class="comment mglr4 activeBox">
			<view class="child">
				<view class="ftw pdb5">参观大唐芙蓉园</view>
				<view class="fs12 color6">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了规划设计法海跟is干活换个飞机多少个扩大复合弓鹤骨鸡肤大好时光</view>
				<view class="imgbox">
					<view class="img lisThree">
						<image src="../../static/images/the-order-img.png" mode=""></image>
					</view>
					<view class="img lisThree">
						<image src="../../static/images/the-order-img.png" mode=""></image>
					</view>
				</view>
				<view class="label pdt15 flexEnd fs13">
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
		<view class="f5H5"></view>
		<view class="pdlr4 pdb10">
			<view class="ftw pdt15 pdb10">参与条件</view>
			<view class="fs12 color6 xqInfor">
				<view>1、回复健康的撒花四氯化硅鹤骨鸡肤商户管理挂号费世界观和规划局分是价格付款苏广加了加工费老岁接个了加工费克鲁赛德就过来监管科廊坊市寄过来</view>
				<view>2、挂号费了撒谎过份了和估计富商大贾和更进反馈单身公害挂号费就开始干活规划局分开始的话购房人几十个热干活</view>
				<view>3、刚发的广告费九十点了光和热和鬼斧神工和公交卡</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="pdlr4">
			<view class="flexRowBetween pdtb15 borderB1">
				<view class="fs14">开始时间</view>
				<view class="fs13">2019年12月12日8点</view>
			</view>
			<view class="flexRowBetween pdtb15">
				<view class="fs14">活动地址</view>
				<view class="fs13">西安市长安区韦曲街道</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="pdlr4">
			<view class="ftw pdt15 pdb10">报名人数<text class="fs12 ftn">(12)</text></view>
			<view class="peopleNum flex fs12">
				<view class="item flex" v-for="(item,index) in peopleNumData" :key="index">
					<view><image class="photo" src="../../static/images/xiaoxi-img.png" mode=""></image></view>
					<view class="name">汪小白</view>
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
				peopleNumData:[{},{},{},{},{},{}]
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
	page{padding-bottom: 20rpx;}
	
	.comment .child{border-bottom: 0;}
	.xqInfor view{margin-bottom: 10rpx;}
	
	.peopleNum{flex-wrap: wrap;}
	.peopleNum .item{width: 45%;margin-bottom: 30rpx;margin-right: 10%;}
	.peopleNum .item:nth-of-type(2n){margin-right: 0;}
	.peopleNum .item .photo{width: 70rpx;height: 70rpx;display: block;border-radius: 50%;}
	.peopleNum .item .name{width:200rpx;overflow: hidden;margin-left: 20rpx;}
</style>
