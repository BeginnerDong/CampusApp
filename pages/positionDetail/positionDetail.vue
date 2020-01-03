<template>
	<view>
		
		<!-- 职位招聘 -->
		<view class="mglr4 pdt20 pdb15">
			<view class="fs18 flexRowBetween mgb15">会计助理</view>
			<view class="flex fs13">
				<view class="red">4k-6k</view>/西安/1-3年&nbsp;/&nbsp;学历不限
			</view>
			<view class="fs12 color6 mgt5">职位诱惑：年底分红13薪</view>
		</view>
		<view class="f5H5"></view>
		
		<!-- 公司名称 -->
		<view class="pdlr4 compyCont">
			<view class="pdt15 pdb10 flexRowBetween">
				<view class="ll">
					<view class="ftn pdb5">创天互娱成都分公司</view>
					<p class="fs12 color6">不需要融资&nbsp;/&nbsp;50-100人&nbsp;/&nbsp;有戏</p>
				</view>
				<view class="rr flexEnd">
					<image class="compyLogo" src="../../static/images/join-us-img.png" />
				</view>
			</view>
			<view class="pdb15 adrss">
				<view class="flex adrssTit fs13"><image class="adrsIcon mgr5" src="../../static/images/join-us-icon1.png"/>西安市雁塔区中投国际A座1802室</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<!-- 职位描述 -->
		<view class="mglr4 pdtb20">
			<view class="fs16 pdb15 ftw">职位描述</view>
			<view class="fs13">职位概述：</view>
			<view class="xqInfor fs12 color6">
				<view>良好的职业道德，强烈的责任岗，会计初级职称，能处理对账、开票、销售回款、账龄分析等业务，能独立制作销售周报、月报工作。</view>
			</view>
			
			<view class="fs13 pdt15 pdt5">任职要求：</view>
			<view class="xqInfor fs12 color6">
				<view>1、良好的职业道德，强烈的责任感，会计初级职称。</view>
				<view>2、能处理好对账、开票、销售回款、账龄分析等业务，能独立制作销售周报、月报工作。</view>
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
				positionDate:[{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				var callback = function(res){
				    console.log('getMainData', res);
				    self.mainData.push.apply(self.mainData,res.info.data);		        
				};
				self.$apis.orderGet(postData, callback);
			}
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

