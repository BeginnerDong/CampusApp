<template>
	<view>
		
		<view class="myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">昵称</view>
				<view class="rr"><input type="text" value="张丹" placeholder="" placeholder-class="placeholder" /></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">性别</view>
				<view class="rr"><input type="text" value="男" placeholder="" placeholder-class="placeholder" /></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">学校</view>
				<view class="rr"><input type="text" value="西安交通大学" placeholder="" placeholder-class="placeholder" /></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">学历</view>
				<view class="rr"><input type="text" value="本科" placeholder="" placeholder-class="placeholder" /></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">入学时间</view>
				<view class="rr"><input type="text" value="2019.09.01" placeholder="" placeholder-class="placeholder" /></view>
			</view>
		</view>
		
		<view class="mglr4 pdtb15 flex">
			<view class="gzBtn fs12 white center" style="width: 140rpx;background: #666;"  @click="Router.navigateTo({route:{path:'/pages/user-realname/user-realname'}})">实名认证</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 150rpx;">
			<button class="btn" type="button">保存</button>
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
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	.myRowBetween .item{padding: 30rpx 4%;}
</style>
