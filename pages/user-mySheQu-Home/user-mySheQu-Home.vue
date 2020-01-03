<template>
	<view>
		
		<view class="whiteBj pdlr4 pdt15 pdb15">
			<view class="ftw">社区人数<text class="fs12 ftn">(12)</text></view>
			<view class="flex SQPerNum pdt10">
				<view class="photo" v-for="(item,index) in SQPerNum" :key="index" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/xiaoxi-img.png" mode=""></image></view>
				<view class="photo" @click="Router.navigateTo({route:{path:'/pages/user-mySheQu-peopleNum/user-mySheQu-peopleNum'}})"><image src="../../static/images/community-icon01.png" mode=""></image></view>
			</view>
		</view>
		
		
		<view class="mglr4 userHomeLis mgt15">
			<view class="item flexRowBetween" v-for="(item,index) in userHomeDate" :key="index" >
				<view class="data">
					<view class="year">2018</view>
					<view class="day">12/27</view>
				</view>
				<view class="infor">
					<view class="list flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
						<view class="pic"><image src="../../static/images/gerenzhuye-img1.png" mode=""></image></view>
						<view class="rr">
							<view class="pdt5 pdb5 avoidOverflow">我在国外做博后</view>
							<view class="fs12 color6 avoidOverflow2">刚发的还是个加工费单身公害感觉粉色看看经历过价格付款的刚发的国防生的刚</view>
						</view>
					</view>
					<view class="list flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
						<view class="pic"><image src="../../static/images/home-img2.png" mode=""></image></view>
						<view class="rr">
							<view class="pdt5 pdb5 avoidOverflow">我在国外做博后</view>
							<view class="fs12 color6 avoidOverflow2">刚发的还是个加工费单身公害感觉粉色看看经历过价格付款的刚发的国防生的刚</view>
						</view>
					</view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="data">
					<view class="year">2018</view>
					<view class="day">12/27</view>
				</view>
				<view class="infor">
					<view class="list"  @click="Router.navigateTo({route:{path:'/pages/postDetails-Two/postDetails-Two'}})">
						<view class="">
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
				SQPerNum:[{},{},{},{},{}],
				userHomeDate:[{},{}]
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
	.SQPerNum .photo{width: 80rpx;height: 80rpx;margin-right: 40rpx;}
	.SQPerNum .photo image{width: 100%;height: 100%;border-radius: 50%;}
	.SQPerNum .photo:last-child{margin-right: 0;}
</style>
