<template>
	<view>
		
		<view class="pdlr4 whiteBj">
			<view class="flexRowBetween pdtb10">
				<view @click="prev()"><image class="backIcon" style="margin-left: 0;" src="../../static/images/arrowL2.png" mode=""></image></view>
				<view class="gzBtn fs12 center white" style="background: #666;">确定</view>
			</view>
		</view>
		<view class="pdlr4 mgt15">
			<view class="pdb10" style="border-bottom: 1px solid #ddd;">
				<input type="text" value="学生·在读" placeholder="请输入" placeholder-class="placeholder">
			</view>
			<view class="pdtb10 fs12 color9">更加准备的让别人了解你。</view>
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
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			prev(){
				const self = this;
				self.$router.go(-1)
			}
		}
	};
</script>

<style>
	@import "../../assets/style/userHome.css";
	page{background: #F5F5F5;}
</style>
