<template>
	<view>
		<view class="orderNav flexRowBetween whiteBj color6 W-Fixed borderB1" style="top: 88rpx;">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">动态</view>
			<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">活动</view>
			<view class="tt flexCenter" :class="curr==3?'on':''" @click="changeCurr('3')">社区</view>
			<view class="tt flexCenter" :class="curr==4?'on':''" @click="changeCurr('4')">评论</view>
		</view>
		<view style="height: 82rpx;"></view>
		
		<view class="mglr4">
			<!-- 动态 -->
			<view class="comment" v-show="curr==1">
				<view class="child" v-for="(item,index) in fabuData" :key="index">
					<view class="fs12">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
					<view class="imgbox">
						<view class="img lisThree">
							<image src="../../static/images/the-store-img2.png" mode=""></image>
						</view>
						<view class="img lisThree">
							<image src="../../static/images/the-store-img2.png" mode=""></image>
						</view>
						<view class="img lisThree">
							<image src="../../static/images/the-store-img2.png" mode=""></image>
						</view>
					</view>
					<view class="fs12 color6 pdt10">2020.01.02</view>
					<view class="label pdt10 flexEnd fs13">
						<view class="lis flex">
							<image src="../../static/images/home-icon3.png" mode=""></image>
							<view>265</view>
						</view>
						<view class="lis flex">
							<image src="../../static/images/home-icon6.png" mode=""></image>
							<view>1562</view>
						</view>
					</view>
				</view>
			</view>	
			
			<!-- 活动 -->
			<view class="comment activeBox" v-show="curr==2" >
				<view class="child pr oh"  v-for="(item,index) in fabuData" :key="index">
					<view @click="Router.navigateTo({route:{path:'/pages/myFabu-activityDetail/myFabu-activityDetail'}})">
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
					</view>
					<view class="flexRowBetween pdt15 fs12">
						<view class="color6">2020.01.02</view>
						<view class="label flexEnd">
							<view class="lis flex">
								<image src="../../static/images/activity-icon2.png" mode=""></image>
								<view>收藏</view>
							</view>
							<view class="lis flex">
								<image src="../../static/images/home-icon6.png" mode=""></image>
								<view>1562</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 社区 -->
			<view class="comment" v-show="curr==3">
				<view class="child" v-for="(item,index) in fabuData" :key="index">
					<view class="flexRowBetween pdb10">
						<view class="flex">
							<view class="photo"><image src="../../static/images/home-img1.png" mode=""></image></view>
							<view class="name">
								<view class="fs12">小白</view>
								<view class="fs10 color6">2019.12.25 16:31</view>
							</view>
						</view>
					</view>
					<view class="fs12">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
					<view class="imgbox">
						<view class="img lisThree">
							<image src="../../static/images/the-store-img2.png" mode=""></image>
						</view>
						<view class="img lisThree">
							<image src="../../static/images/the-store-img2.png" mode=""></image>
						</view>
						<view class="img lisThree">
							<image src="../../static/images/the-store-img2.png" mode=""></image>
						</view>
					</view>
					<view class="fs12 color6 pdt10">2020.01.02</view>
					<view class="label pdt15 flexEnd fs13">
						<view class="lis flex">
							<image src="../../static/images/home-icon4.png" mode=""></image>
							<view>18</view>
						</view>
						<view class="lis flex">
							<image src="../../static/images/home-icon3.png" mode=""></image>
							<view>265</view>
						</view>
						<view class="lis flex">
							<image src="../../static/images/home-icon6.png" mode=""></image>
							<view>1562</view>
						</view>
					</view>
				</view>
			</view>	
			
			<!-- 评论 -->
			<view class="zanCont" v-show="curr==4">
				<view class="item" v-for="(item,index) in fabuData" :key="index">
					<view class="pdb10 fs13">和公交卡发烧了火锅料和国际法大使馆换个方式集点卡很过分供货方时大家开个会</view>
					<view class="cont f5bj flex">
						<view class="pic"><image src="../../static/images/the-message-img1.png" mode=""></image></view>
						<view class="rr">
							<view class="tit fs13 avoidOverflow">标题标题标题标题标题标题标题标题</view>
							<view class="text avoidOverflow2 fs12 color6">双菇还记得老汉还说加工费几点过来价格付款落地生根加工费回户加工费点了结果火锅店机构管理很反感华海景房</view>
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
				is_show:false,
				curr:1,
				fabuData:[{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/comment.css";
	@import "../../assets/style/zanCont.css";
	page{padding-bottom: 60rpx;}
	
	.zanCont .item{padding: 30rpx 0;}
</style>
