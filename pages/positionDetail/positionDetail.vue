<template>
	<view>
		
		<!-- 职位招聘 -->
		<view class="mglr4 pdt20 pdb15">
			<view class="fs18 flexRowBetween mgb15">{{mainData.title}}</view>
			<view class="flex fs13">
				<view class="red">{{mainData.salary}}</view>/{{mainData.city}}/{{mainData.experience}}&nbsp;/&nbsp;{{mainData.education}}
			</view>
			<view class="fs12 color6 mgt5">{{mainData.description}}</view>
		</view>
		<view class="f5H5"></view>
		
		<!-- 公司名称 -->
		<view class="pdlr4 compyCont">
			<view class="pdt15 pdb10 flexRowBetween">
				<view class="ll">
					<view class="ftn pdb5">{{mainData.company}}</view>
					<p class="fs12 color6">{{mainData.finance}}&nbsp;/&nbsp;{{mainData.scale}}&nbsp;/&nbsp;{{mainData.industry}}</p>
				</view>
				<view class="rr flexEnd">
					<image class="compyLogo" :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" />
				</view>
			</view>
			<view class="pdb15 adrss">
				<view class="flex adrssTit fs13"><image class="adrsIcon mgr5" src="../../static/images/join-us-icon1.png"/>{{mainData.address}}</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<!-- 职位描述 -->
		<view class="mglr4 pdtb20">
			<view class="fs16 pdb15 ftw">职位描述</view>
			<view class="content ql-editor" style="padding:0;"
			v-html="mainData.content">
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="mglr4 pdt20 pdb25 flexCenter center">
			<view style="width: 500rpx;line-height: 50rpx;">
				<view class="fs13">如果您对个人职位有意向，请将个人简历投递至我们邮箱</view>
				<view class="fs15 ftw pdt5">568956236@qq.com</view>
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
				positionDate:[{},{},{}],
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		},
	}
</script>


<style>
	
	.compyCont .ll{width: 80%;}
	.compyCont .rr{width: 20%;}
	.compyCont .compyLogo{width:50px; height:50px; display: block;border-radius: 50%;}
	
	.awardList{flex-wrap: wrap;}
	.awardList .item{ width: 30%; line-height: 30px; height: 30px;border-radius: 4px;background:#ecf4ff ;margin:0 5% 10px 0;}
	.awardList .item:nth-of-type(3n){margin-right: 0;}
	.adrsIcon{width: 20rpx;height: 24rpx;display: block;}
	
	/* 职位描述 */
	.xqInfor view{margin-top: 10rpx;}
	
	
</style>

