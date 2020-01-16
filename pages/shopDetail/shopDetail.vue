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
							<view class="photo"><image src="../../static/images/home-img1.png" mode=""></image></view>
							<view class="name">
								<view class="fs12">小白</view>
								<view class="fs10 color6">2019.12.25 16:31</view>
							</view>
						</view>
						<view class="flexEnd">
							<view class="starBox mgr5">
								<image src="../../static/images/the-store-icon4.png" mode=""></image>
								<image src="../../static/images/the-store-icon4.png" mode=""></image>
								<image src="../../static/images/the-store-icon4.png" mode=""></image>
								<image src="../../static/images/the-store-icon4.png" mode=""></image>
								<image src="../../static/images/the-store-icon5.png" mode=""></image>
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
				</view>
			</view>
			<view class="comment" v-if="mainData.message&&mainData.message.length==0">
				暂无评价~
			</view>
			<view class="pdtb15 flexCenter" v-if="mainData.message&&mainData.message.length>2" @click="Router.navigateTo({route:{path:'/pages/shop-comment/shop-comment'}})">
				<view class="flexCenter lookAll-btn fs12">查看全部 <image class="arrowR" src="../../static/images/about-icon1.png" mode=""></image></view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<!-- 商家信息 -->
		<view class="pdlr4">
			<view class="pdt15 ftw">商家信息</view>
			<view class="fs13 color6 pdtb10">就分了的价格哈根flag哈拉嘎嘎监管科撒寄过来人家给感叹号给贴梗海棠让大哥花费案发后及hihi合法化反和范德萨发货后方可卡皇冠假日。</view>
			<view>
				<image style="width: 400rpx;height: 480rpx;" src="../../static/images/the-store-img3.png" mode=""></image>
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
				mainData:{}
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
