<template>
	<view>
		
		<view class="pdlr4">
			<view class="dialogBox">
				<view class="item left">
					<view class="time center fs13 color9">19-12-27 14:23</view>
					<view class="infor">
						<image class="Photo" src="../../static/images/xiaoxi-img.png" >
						<view class="text">你好，想和你详细沟通下，现在方便吗？非的行列读后感 割发代首家里给会返利是这个和磊哥换个方式的尖括号割发代首开了个会了过节费肯定是联合国干活附近的干活附近的看更进反馈多少个一缸发动机开始估红富士康</view>
					</view>
				</view>
				<view class="item right">
					<view class="time center fs13 color9">14:23</view>
					<view class="infor">
						<view class="text">你好，我已有1年的工作经验，现任职位会计助理，对贵公司该职位很感兴趣，希望能与你进一步沟通。海景房卡商量过了和公交卡发送到很尴尬交货更快莲富大厦个帅哥和公交卡粉色的交货更快换个飞机是</view>
						<image class="Photo" src="../../static/images/the-message-img.png" >
					</view>
				</view>
			</view>
		</view>
		
		<view class="fix-talkIput flexRowBetween whiteBj fs13">
			<view class="input">
				<input type="text" placeholder="" placeholder-class="placeholder">
			</view>
			<view class="flexEnd">
				<button class="send fs13" type="button">发送</button>
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
				is_show:false
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
	@import "../../assets/style/tidings.css";
	page{padding-bottom: 130rpx;background: #F5F5F5;}
	
	
</style>

