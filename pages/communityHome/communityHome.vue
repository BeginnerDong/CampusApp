<template>
	<view>
		
		<view class="userHead whiteBj">
			<view class="pdt25 pdlr4 flexRowBetween">
				<view class="flex" style="width:85%;">
					<image class="photo" src="../../static/images/home-img.png" mode=""></image>
					<view style="width: 70%;">
						<view class="fs16">(考研社区)</view>
					</view>
				</view>
				<view class="gzBtn fs12 center white" style="background: #666;">加入</view>
			</view>
		</view>
		<view class="whiteBj pdlr4 fs13 color6">
			<view class="pdtb15"><span class="ftw color2">简介：</span>简介介绍激发和立法会发好几个范德萨发龙岗店啦国家级卡里估计家里的事结了婚噶的框架加工费但是看</view>
		</view>
		
		<view class="fabubtn" @click="Router.navigateTo({route:{path:'/pages/communityFaBu/communityFaBu'}})"><image class="icon" src="../../static/images/home-icon1.png" mode=""></image></view>
		
		<view class="mglr4 userHomeLis mgt15">
			<view class="item flexRowBetween" v-for="(item,index) in userHomeDate" :key="index">
				<view class="data">
					<view class="year">2018</view>
					<view class="day">12/27</view>
				</view>
				<view class="infor">
					<view class="list flexRowBetween">
						<view class="pic"><image src="../../static/images/gerenzhuye-img1.png" mode=""></image></view>
						<view class="rr">
							<view class="pdt5 pdb5 avoidOverflow">我在国外做博后</view>
							<view class="fs12 color6 avoidOverflow2">刚发的还是个加工费单身公害感觉粉色看看经历过价格付款的刚发的国防生的刚</view>
						</view>
					</view>
					<view class="list flexRowBetween">
						<view class="pic"><image src="../../static/images/home-img2.png" mode=""></image></view>
						<view class="rr">
							<view class="pdt5 pdb5 avoidOverflow">我在国外做博后</view>
							<view class="fs12 color6 avoidOverflow2">刚发的还是个加工费单身公害感觉粉色看看经历过价格付款的刚发的国防生的刚</view>
						</view>
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
				score:'',
				wx_info:{},
				userHomeDate:[{},{},{}]
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
	@import "../../assets/style/userHome.css";
	page{background: #F5F5F5;}
	.userHead{}
</style>
