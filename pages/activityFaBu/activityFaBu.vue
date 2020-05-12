<template>
	<view>
		<view class="myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">活动标题</view>
				<view class="rr"><input type="text" v-model="submitData.title" placeholder="请输入" placeholder-class="placeholder" /></view>
			</view>
			<view class="item" style="border-bottom: 0;">
				<textarea v-model="submitData.content" placeholder="请输入你想说的话" placeholder-class="placeholder"/>
			</view>
			
			<view class="item">
				<view class="flex" style="flex-wrap: wrap;">
					<block v-for="(item,index) in submitData.mainImg" :key="index">
						<view class="upBtn">
							<image :src="item.url"  @tap="previewImage"></image>
						</view>
					</block>
					<view class="upBtn" @tap="upLoadImg('mainImg')">
						<image src="../../static/images/release-icon.png" mode=""></image>
					</view>
					<view class="mgl15 color9 fs11">可上传多张图片</view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">起始时间</view>
				<view class="rr">
					<picker mode="date"  @change="dateChange">
						<view class="flex fs13 color6 ">
							<view>{{time!=''?time:'请选择'}}</view>
							<view class="mgl10">
								<image class="jiaBtn" src="../../static/images/release-icon1.png" mode=""></image>
							</view>
						</view>
						
					</picker>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">发起人</view>
				<view class="rr fs13">
					<view class="flex mgr25" @click="setChange('1','与官方合作')">
						<image class="setIcon" :src="seltCurr==1?'../../static/images/nipbanli-icon.png':'../../static/images/nipbanli-icon1.png'" mode=""></image>
						<text>与官方合作</text>
					</view>
					<view class="flex" @click="setChange('2','个人')">
						<image class="setIcon" :src="seltCurr==2?'../../static/images/nipbanli-icon.png':'../../static/images/nipbanli-icon1.png'" mode=""></image>
						<text>个人</text>
					</view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">联系方式</view>
				<view class="rr"><input type="number" maxlength="11" v-model="submitData.phone" placeholder="请输入" placeholder-class="placeholder" /></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">活动位置</view>
				<view class="rr">
					<view class="flexEnd"  @click="weizhiShow" :style="submitData.position==''?'color:#999':''">
						{{submitData.position!=''?submitData.position:'请选择'}}
						<view class="mgl10">
							<image class="jiaBtn" src="../../static/images/release-icon1.png" mode=""></image>
						</view>
					</view>
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
				<view class="rr"><input type="text" v-model="submitData.address" placeholder="请输入" placeholder-class="placeholder" /></view>
			</view>
			<view class="item">
				<view>参与条件</view>
				<view class="mgt15 f5bj" style="padding: 30rpx;">
					<textarea v-model="submitData.condition" placeholder="请输入" placeholder-class="placeholder"/>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">参与人数</view>
				<view class="rr">
					<picker mode="selector" :range="numArray" @change="numChange">
						<view :style="submitData.num==''?'color:#999':''">{{submitData.num!=''?submitData.num:'请选择参与人数'}}</view>
					</picker>
					<!-- <input type="number" v-model="submitData.num" placeholder="请输入" placeholder-class="placeholder" /> -->
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">提升曝光度</view>
				<view class="rr">
					<switch  @change="switch1Change" checked="true" color="#222" style="transform: scale(0.7);"/>
				</view>
			</view>
		</view>
		<view class="submitbtn mgb30" style="margin-top: 100rpx;">
			<button class="btn" type="button" @click="$Utils.stopMultiClick(submit)">发布</button>
		</view>
		
		<view class="black-bj" v-if="is_show" @click="weizhiShow"></view>
		<view class="weizhiShow radius10 whiteBj fs13" v-if="is_weizhiShow">
			<view class="item" @click="choosePosition('室内')">室内</view>
			<view class="item" @click="choosePosition('室外')">室外</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				imageList: [],
				is_show:false,
				seltCurr:0,
				is_weizhiShow:false,
				index:0,
				numArray:['不限','1-10','10-20','20-30','30-40','40-50'],
				array:['西安市','咸阳市','渭南市','铜川市','宝鸡市','汉中市','安康市'],
				submitData:{
					title:'',
					type:2,
					content:'',
					mainImg:[],
					headImg:[],
					start_time:'',
					initiator:'',
					phone:'',
					city:'',
					address:'',
					condition:'',
					num:'',
					listorder:'1',
					name:'',
					position:''
				},
				time:''
			}
		},
		
		
		onLoad() {
			const self = this;
			self.submitData.name=uni.getStorageSync('user_info').info.name;
			self.submitData.headImg=uni.getStorageSync('user_info').info.mainImg;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			numChange(e){
				const self = this;
				self.submitData.num = self.numArray[e.detail.value]
			},
			
			switch1Change: function (e) {
				const self = this;
				console.log('switch1 发生 change 事件，携带值为', e.target.value);
				if(e.target.value==true){
					self.submitData.listorder = 1
				}else{
					self.submitData.listorder = 0
				}
			},
			
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
			
			setChange(seltCurr,item){
				const self = this;
				if(seltCurr!=self.seltCurr){
					self.seltCurr = seltCurr
				}
				self.submitData.initiator = item
			},
			
			weizhiShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_weizhiShow = !self.is_weizhiShow
			},
			
			choosePosition(item){
				const self = this;
				self.submitData.position = item;
				self.is_show = !self.is_show;
				self.is_weizhiShow = !self.is_weizhiShow
			},
			
			
			cityChange(e) {
				// 搜索选择分类
				const self = this;
				self.index = e.target.value;
				self.submitData.city=self.array[self.index];
			},
			
			dateChange(e){
				const self = this;
				self.time = e.detail.value;
				self.submitData.start_time = self.$Utils.timeToTimestamp(self.time);
				console.log(self.time);
				console.log(self.submitData.start_time)
			},
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.mainImg;
				delete newObject.headImg;
				delete newObject.name;
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					self.newsAdd();	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			upLoadImg(type) {
				const self = this;	
				if (self.submitData[type].length > 8) {
					self.$Utils.showToast('仅限9张', 'fail');
					return;
				};
				wx.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log('type',type)
						console.log(self.submitData)
						wx.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 9-self.submitData[type].length,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths;
						console.log(callback)
						for (var i = 0; i < tempFilePaths.length; i++) {
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								tokenFuncName: 'getUserToken'
							}, callback)
						}
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
			newsAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('发送成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.newsAdd(postData, callback);
			},
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
