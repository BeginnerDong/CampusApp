<template>
	<view>
		<view class="pdlr4">
			<view class="fs16 ftw pdt15">呷哺呷哺(西安高新大都荟)</view>
			
			<view class="fs12">
				<view class="flexRowBetween shop-betn">
					<image class="samll-Icon mgr5" src="../../static/images/the-store-icon.png" mode=""></image>
					<view class="rr">营业中 | 周一至周日 10:00-22:00</view>
				</view>
				<view class="flexRowBetween shop-betn">
					<image class="samll-Icon mgr5" src="../../static/images/the-store-icon1.png" mode=""></image>
					<view class="rr avoidOverflow">丈八街道高新四路赵家坡村高新大都荟项目第J栋</view>
				</view>
				<view class="flexRowBetween shop-betn">
					<image class="samll-Icon mgr5" src="../../static/images/coupons-icon2.png" mode=""></image>
					<view class="rr avoidOverflow">15689565623</view>
				</view>
			</view>
			
			
		</view>
		<view class="f5H5"></view>
		
		<view class="pdlr4">
			<view class="pdt15 pdb10 fs13">店铺商品</view>
			<view class="shop_allimg pr pdb10">
				<view class="fix_num fs12 white">共4张</view>
				<scroll-view scroll-x>
					<view class="item"><image src="../../static/images/the-store-img.png" mode=""></image></view>
					<view class="item"><image src="../../static/images/the-store-img.png" mode=""></image></view>
					<view class="item"><image src="../../static/images/the-store-img.png" mode=""></image></view>
					<view class="item"><image src="../../static/images/the-store-img.png" mode=""></image></view>
				</scroll-view>
			</view>
		</view>
		<view class="f5H5"></view>
		<!-- 评价 -->
		<view class="pdlr4">
			<view class="pdt15 ftw">评价<text class="ftn">(345)</text></view>
			<view class="comment">
				<view class="child" v-for="(item,inedx) in pingjiaData" :key="index">
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
			<view class="pdtb15 flexCenter" @click="Router.navigateTo({route:{path:'/pages/shop-comment/shop-comment'}})">
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
				pingjiaData:[{},{}]
			}
		},
		
		onLoad(options) {
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
