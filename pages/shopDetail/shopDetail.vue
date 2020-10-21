<template>
	<view>
		<view class="pdlr4">
			<view class="fs16 ftw pdt15">{{mainData.name}}</view>
			
			<view class="fs12">
				<view class="flexRowBetween shop-betn">
					<image class="samll-Icon mgr5" src="../../static/images/the-store-icon.png" mode=""></image>
					<view class="rr">营业中 | 周一至周日 10:00-22:00</view>
				</view>
				<view class="flexRowBetween shop-betn">
					<image class="samll-Icon mgr5" src="../../static/images/the-store-icon1.png" mode=""></image>
					<view class="rr avoidOverflow">{{mainData.address}}</view>
				</view>
				<view class="flexRowBetween shop-betn">
					<image class="samll-Icon mgr5" src="../../static/images/coupons-icon2.png" mode=""></image>
					<view class="rr avoidOverflow">{{mainData.phone}}</view>
				</view>
			</view>
			
			
		</view>
		<view class="f5H5"></view>
		
		<view class="pdlr4">
			<view class="pdt15 pdb10 fs13">店铺商品</view>
			<view class="shop_allimg pr pdb10">
				<view class="fix_num fs12 white">共{{mainData.product?mainData.product.length:0}}张</view>
				<scroll-view scroll-x>
					<view class="item" v-for="(item,index) in mainData.product">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					
				</scroll-view>
			</view>
		</view>
		<view class="f5H5"></view>
		<!-- 评价 -->
		<view class="pdlr4">
			<view class="pdt15 ftw">评价<text class="ftn">({{mainData.message?mainData.message.length:0}})</text></view>
			<view class="comment" v-if="mainData.message&&mainData.message.length>0">
				<view class="child" v-for="(item,index) in mainData.message" :key="index" v-if="index<2">
					<view class="flexRowBetween">
						<view class="flex">
							<view class="photo"><image :src="item.mainImg&&item.mainImg.length>0?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image></view>
							<view class="name">
								<view class="fs12">{{item.title}}</view>
								<view class="fs10 color6">{{item.create_time}}</view>
							</view>
						</view>
						<view class="flexEnd">
							
							<view class="starBox mgr5">
								<image v-for="(c_item,c_index) in stars" :key="c_index" :src="item.score > c_item ?(item.score/2-c_item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
								
								</image>
							</view>
						</view>
					</view>

					<view class="fs12 pdt10">{{item.description}}</view>
					<view class="imgbox" v-if="item.bannerImg.length>0">
						<view class="img lisThree" v-for="(c_item,c_index) in item.bannerImg" :key="c_index">
							<image :src="item.url" mode=""></image>
						</view>
					</view>
				</view>
			</view>
			<view class="comment" style="line-height: 60rpx;" v-if="mainData.message&&mainData.message.length==0">
				暂无评价~
			</view>
			<view class="pdtb15 flexCenter" v-if="mainData.message&&mainData.message.length>2" 
			@click="Router.navigateTo({route:{path:'/pages/shop-comment/shop-comment?user_no='+self.user_no}})">
				<view class="flexCenter lookAll-btn fs12">查看全部 <image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<!-- 商家信息 -->
		<view class="pdlr4">
			<view class="pdt15 ftw">商家信息</view>
			<view class="fs13 color6 pdtb10">
				<view class="content ql-editor"  v-html="mainData.content">
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
				pingjiaData:[{},{}],
				mainData:{},
				user_no:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.user_no = options.user_no;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: self.user_no,
					user_type:1
				};
				postData.getAfter = {
					product:{
						tableName:'Product',
						middleKey:'user_no',
						key:'shop_no',
						searchItem:{
							status:1
						},
						condition:'=',
					},
					message:{
						tableName:'Message',
						middleKey:'user_no',
						key:'relation_user',
						searchItem:{
							type:4,
							status:1
						},
						condition:'=',
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/star.css";
	@import "../../assets/style/comment.css";
	page{padding-bottom: 60rpx;}
	.samll-Icon{width: 28rpx;height: 28rpx; display: block;}
	.shop_allimg .fix_num{line-height: 40rpx;padding: 0 10rpx;background: rgba(0,0,0,0.5);border-radius: 6rpx;position: absolute; bottom: 40rpx;left: 20rpx; z-index: 2;}
	.shop_allimg scroll-view{white-space: nowrap;}
	.shop_allimg .item{margin-right: 20rpx; display: inline-block;box-sizing: border-box;}
	.shop_allimg .item image{width: 240rpx;height: 160rpx;border-radius: 6rpx;}
	
	.shop-betn .rr{width: 93%; padding: 20rpx 0;border-bottom: 1px solid #eee; }
	.shop-betn:last-child .rr{border-bottom: none;}
	.lookAll-btn{width: 180rpx;line-height: 50rpx;border-radius: 30rpx;border:1px solid #eee;}

</style>
