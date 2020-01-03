<template>
	<view>
		<view class="myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">活动标题</view>
				<view class="rr"><input type="text" value="" placeholder="请输入" placeholder-class="placeholder" /></view>
			</view>
			<view class="item" style="border-bottom: 0;">
				<textarea value="" placeholder="请输入你想说的话" placeholder-class="placeholder"/>
			</view>
			
			<view class="item">
				<view class="flex">
					<view class="upBtn" @tap="chooseImage"><image src="../../static/images/release-icon.png" mode=""></image></view>
					<view class="mgl15 color9 fs11">可上传多张图片</view>
				</view>
				<view class="flex" style="flex-wrap: wrap;">
					<block v-for="(image,index) in imageList" :key="index">
						<view class="upImgLis">
							<image :src="image" :data-src="image" @tap="previewImage"></image>
						</view>
					</block>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">起始时间</view>
				<view class="rr">
					<view class="flexEnd"><image class="jiaBtn" src="../../static/images/release-icon1.png" mode=""></image></view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">发起人</view>
				<view class="rr fs13">
					<view class="flex mgr25" @click="setChange('1')">
						<image class="setIcon" :src="seltCurr==1?'../../static/images/nipbanli-icon.png':'../../static/images/nipbanli-icon1.png'" mode=""></image>
						<text>与官方合作</text>
					</view>
					<view class="flex" @click="setChange('2')">
						<image class="setIcon" :src="seltCurr==2?'../../static/images/nipbanli-icon.png':'../../static/images/nipbanli-icon1.png'" mode=""></image>
						<text>个人</text>
					</view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">联系方式</view>
				<view class="rr"><input type="text" value="" placeholder="请输入" placeholder-class="placeholder" /></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">活动位置</view>
				<view class="rr">
					<view class="flexEnd"  @click="weizhiShow"><image class="jiaBtn" src="../../static/images/release-icon1.png" mode=""></image></view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">选择城市</view>
				<view class="rr">
					<picker @change="cityChange" :value="index" :range="array">
						<view class="flex fs13 color6 ">
							<view>{{array[index]}}</view>
							<view class="mgl10">
								<image class="jiaBtn" src="../../static/images/release-icon1.png" mode=""></image>
							</view>
						</view>
					</picker>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">详细地址</view>
				<view class="rr"><input type="text" value="" placeholder="请输入" placeholder-class="placeholder" /></view>
			</view>
			<view class="item">
				<view>参与条件</view>
				<view class="mgt15 f5bj" style="padding: 30rpx;">
					<textarea value="" placeholder="请输入" placeholder-class="placeholder"/>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">参与人数</view>
				<view class="rr"><input type="number" value="" placeholder="请输入" placeholder-class="placeholder" /></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">提升曝光度</view>
				<view class="rr">
					<switch checked="true" color="#222" style="transform: scale(0.7);"/>
				</view>
			</view>
		</view>
		<view class="submitbtn mgb30" style="margin-top: 100rpx;">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/index/index'}})">发布</button>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="weizhiShow radius10 whiteBj fs13" v-show="is_weizhiShow">
			<view class="item" @click="weizhiShow">室内</view>
			<view class="item" @click="weizhiShow">室外</view>
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
				imageList: [],
				seltCurr:0,
				is_weizhiShow:false,
				index:0,
				array:['西安市','咸阳市','渭南市','铜川市','宝鸡市','汉中市','安康市']
			}
		},
		onUnload() {
			this.imageList = []
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			chooseImage: async function() {
				uni.chooseImage({
					success: (res) => {
						this.imageList = this.imageList.concat(res.tempFilePaths);
					}
				})
			},
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
			setChange(seltCurr){
				const self = this;
				if(seltCurr!=self.seltCurr){
					self.seltCurr = seltCurr
				}
			},
			weizhiShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_weizhiShow = !self.is_weizhiShow
			},
			cityChange(e) {
				// 搜索选择分类
				const self = this;
				self.index = e.target.value
			},
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
	@import "../../assets/style/editInfor.css";
	.myRowBetween .item{padding: 30rpx 4%;}
	.myRowBetween .item .ll{width: 40%;}
	.myRowBetween .item .rr{width: 60%;}
	.myRowBetween .item textarea{padding: 0;border: 0;}
	.upBtn{width: 120rpx;height: 120rpx;background: #F5F5F5;margin: 0 18rpx 20rpx 0;}
	.upBtn image{width: 100%;height: 100%; display: block;}
	
	.upImgLis{width: 156rpx;height: 156rpx;background: #F5F5F5;margin: 0 20rpx 20rpx 0;}
	.upImgLis:nth-of-type(4n){margin-right: 0;}
	.upImgLis image{width: 100%;height: 100%; display: block;}
	
	.jiaBtn{width: 26rpx; height: 26rpx;display: block;}
	.setIcon{width: 32rpx;height: 32rpx;display: block;margin-right: 10rpx;}
	
	.weizhiShow{width: 80%;position: fixed; top: 50%;left: 50%;z-index: 50;transform: translate(-50%,-50%);}
	.weizhiShow .item{padding: 30rpx 4%;border-bottom: 1px solid #eee;}
	.weizhiShow .item:last-child{border-bottom: 0;}
</style>
