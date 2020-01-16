<template>
	<view>
		
		<view class="mglr4">
			<view class="CouponCard">
				<view class="item">
					<view class="fs10">{{mainData.title}}</view>
					<view class="fs16 pdt5 pdb10 ftw">{{mainData.description}}</view>
					<view class="xian"></view>
					<view class="flexRowBetween pdt10 fs11">
						<view>{{mainData.passage1}}</view>
						<view class="fs10 color9">{{mainData.passage2}}</view>
					</view>
				</view>
			</view>
			
			<view class="pdlr4 pdt15 pdb15 radius10 whiteBj mgt15">
				<view class="">使用规则</view>
				<view class="xqInfor fs12">
					<view class="content ql-editor"  v-html="mainData.content">
					</view>
				</view>
				<view class="pdt10 borderB1"></view>
				<view class="flexEnd fs12 pdt10">更多规则<image class="arrowB" src="../../static/images/coupons-icon1.png" mode=""></image></view>
			</view>
			
			<view class="pdlr4 pdt15 pdb15 radius10 whiteBj mgt15 canTing">
				<view class="pdb10">餐厅介绍</view>
				<view class="flex" @click="Router.redirectTo({route:{path:'/pages/shopDetail/shopDetail?user_no='+mainData.shop_no}})">
					<view class="pic">
						<image :src="mainData.shop&&mainData.shop[0]&&mainData.shop[0].mainImg
						&&mainData.shop[0].mainImg[0]?mainData.shop[0].mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor mgl10  fs12 color6" style="width: 70%;">
						<view class="fs14 color2 ftw">{{mainData.shop&&mainData.shop[0]?mainData.shop[0].name:''}}</view>
						<!-- <view class="flex" style="flex-wrap: wrap;">
							<view class="lable">甜点</view>
						</view> -->
						<view class="adrs mgt15">{{mainData.shop&&mainData.shop[0]?mainData.shop[0].address:''}}</view>
					</view>
				</view>
			</view>
			
			<!-- 底部菜单按钮 -->
			<view class="xqbotomBar flexRowBetween">
				<view class="left price fs16">{{mainData.price}}</view>
				<view class="payBtn pubBj white" v-if="userInfoData.check_status==2" @click="Router.navigateTo({route:{path:'/pages/food-orderConfirm/food-orderConfirm?id='+mainData.id}})">立即抢购</view>
				<view class="payBtn pubBj white" v-if="userInfoData.check_status!=2" @click="realnameshow">立即抢购</view>
			</view>
			
			<view class="black-bj" v-show="is_show"></view>
			
			<view class="realnameshow whiteBj radius10 pdb25" v-show="is_realnameshow">
				<view class="closebtn" style="z-index: 2;" @click="realnameshow">×</view>
				<view>
					<image class="bjImg w" src="../../static/images/about-icon2.png" mode="widthFix"></image>
				</view>
				<view class="pdt25 pdb15 center fs18 ftw">实名认证</view>
				<view class="fs12 pdl20 pdr20">您还未认证身份信息，为方便您使用与享受更多的特权服务，请前去认证身份，完善资料。</view>
				<view class="submitbtn mgt25 mgb25" @click="Router.navigateTo({route:{path:'/pages/user-realname/user-realname'}})">
					<button class="btn">立即实名</button>
				</view>
				<view class="center color9 mgb10" @click="realnameshow">下次验证</view>
				
			</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				userInfoData:{},
				is_show:false,
				is_realnameshow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getUserInfoData','getMainData'], self);
		},
		
		methods: {
			
			realnameshow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_realnameshow = !self.is_realnameshow
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.getAfter = {
					shop:{
						token:uni.getStorageSync('user_token'),
						tableName:'UserInfo',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
					}
				};
				postData.searchItem = {
					thirdapp_id:2,
					id:self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/foodDetail.css";
	
	page{padding-bottom:150rpx;background: #F5F5F5;}
	.xqInfor view{margin-top: 10rpx;}
	.realnameshow{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 42;}
	.realnameshow .bjImg{width: 100%;display: block;}
</style>
