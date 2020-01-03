<template>
	<view>
		<view class="orderNav flexRowBetween whiteBj color6 W-Fixed borderB1" style="top: 88rpx;">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">动态</view>
			<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">活动</view>
			<view class="tt flexCenter" :class="curr==3?'on':''" @click="changeCurr('3')">社区</view>
		</view>
		<view style="height: 82rpx;"></view>
		
		<view class="mglr4">
			<!-- 动态 -->
			<view class="comment" v-show="curr==1">
				<view class="child" v-for="(item,index) in collectData" :key="index">
					<view class="flex">
						<view class="photo"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
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
					<view class="label pdt15 flexEnd fs13">
						<view class="lis flex">
							<image src="../../static/images/home-icon3.png" mode=""></image>
							<view>265</view>
						</view>
						<view class="lis flex">
							<image src="../../static/images/home-icon5.png" mode=""></image>
							<view>1562</view>
						</view>
					</view>
				</view>
			</view>	
			
			<!-- 活动 -->
			<view class="comment activeBox" v-show="curr==2" >
				<view class="child pr oh"  v-for="(item,index) in collectData" :key="index">
					<view class="flex">
						<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
						<view class="name">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019.12.25 16:31</view>
						</view>
					</view>
					<view class="ftw pdt10 pdb5">参观大唐芙蓉园</view>
					<view class="fs12 color6">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了规划设计法海跟is干活换个飞机多少个扩大复合弓鹤骨鸡肤大好时光</view>
					<view class="imgbox">
						<view class="img lisThree">
							<image src="../../static/images/the-order-img.png" mode=""></image>
						</view>
						<view class="img lisThree">
							<image src="../../static/images/the-order-img.png" mode=""></image>
						</view>
					</view>
					<view class="label flexEnd pdt15 fs12">
						<view class="lis flex">
							<image src="../../static/images/activity-icon3.png" mode=""></image>
							<view>收藏</view>
						</view>
						<view class="lis flex">
							<image src="../../static/images/home-icon5.png" mode=""></image>
							<view>1562</view>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 社区 -->
			<view class="comment" v-show="curr==3">
				<view class="child">
					<view class="flexRowBetween">
						<view class="flex">
							<view class="photo" @click="Router.navigateTo({route:{path:'/pages/userHome/userHome'}})"><image src="../../static/images/home-img1.png" mode=""></image></view>
							<view class="name">
								<view class="fs12">小白</view>
								<view class="fs10 color6">2019.12.25 16:31</view>
							</view>
						</view>
						<view class="pr">
							<view class="flexEnd" @click="shareBtnShow">
								<view class="dian"></view>
								<view class="dian"></view>
								<view class="dian"></view>
							</view>
							<view class="fx-shareBtn fs11 white" v-show="is_shareBtnShow">
								<view class="flexCenter pdb10" @click="like">
									<image class="icon" :src="is_like==true?'../../static/images/home-icon9.png':'../../static/images/home-icon7.png'" mode=""></image>
									<text>收藏</text>
								</view>
								<view class="flexCenter" @click="Router.navigateTo({route:{path:'/pages/report/report'}})">
									<image class="icon" src="../../static/images/home-icon8.png" mode=""></image>
									<text>举报</text>
								</view>
							</view>
						</view>
					</view>
					<view class="fs12 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
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
							<image src="../../static/images/home-icon5.png" mode=""></image>
							<view>1562</view>
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
				collectData:[{},{}],
				is_shareBtnShow:false,
				is_like:false
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
			},
			shareBtnShow(){
				const self = this;
				self.is_shareBtnShow = !self.is_shareBtnShow
			},
			like(){
				const self = this;
				self.is_like = !self.is_like;
				self.is_shareBtnShow = !self.is_shareBtnShow
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/comment.css";
	@import "../../assets/style/postDetail.css";
	page{padding-bottom: 60rpx;}
	
	.zanCont .item{padding: 30rpx 0;}
</style>
