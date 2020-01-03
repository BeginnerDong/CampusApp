<template>
	<view>
		<view class="">
			<view class="signInHead center">
				<view class="white fs13 mgb15">NIP值</view>
				<view class="bigNum">20</view>
			</view>
			
			<view class="boxTwo boxShaow mglr4 whiteBj radius10 mgb20">
				<view class="flexRowBetween mgb20">
					<view class="fs13">今日可签到获得<text class="red">88NIP</text></view>
					<view class="radsBtnH50 fs10"  @click="alertShow">立即签到</view>
				</view>
				<view class="dayNIP flex fs12">
					<view class="item flexColumn" v-for="(item,index) in dayNIPData" :key="index">
						<view class="num flexCenter">+{{item.num}}</view>
						<view class="color6">{{item.day}}天</view>
					</view>
				</view>
			</view>
			
			<sw-calendar @tapDate="tapDate"></sw-calendar>
			
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="alertShow" v-show="is_alertShow">
			<view class="closeBtn" @click="alertShow">×</view>
			<view class="flexCenter"><image class="fudaiIcon" src="../../static/images/sign-in-img.png" mode=""></image></view>
			<view class="white mgt25 center fs13">恭喜您获得<span class="num ftw">88</span>NIP</view>
		</view>
	</view>
</template>

<script>
	import swCalendar from '@/components/sw-calendar/sw-calendar.vue';
	// import swCalendar from '@/components/uni-calendar/uni-calendar.vue';
	export default {
		components:{
			swCalendar
		},
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				dayNIPData:[
					{num:28,day:'1'},
					{num:38,day:'2'},
					{num:48,day:'3'},
					{num:58,day:'4'},
					{num:68,day:'5'},
					{num:78,day:'6'},
					{num:88,day:'7'}
				],
				is_alertShow:false,
				year: 2020,
				month: 1,
				date: 3,
				type:'today' //today-今天 pre-今天之前 next-今天之后
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			alertShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_alertShow = !self.is_alertShow;
			},
			tapDate (data) {
			//根据data.type进行判断用户点击的是哪一天,进行下一步业务操作
				console.log(data)
			}
		}
	}
</script>

<style>
	
	page{padding-bottom: 60rpx;}
	.signInHead{width: 100%;height: 400rpx;background-image: linear-gradient(to right, #190015,#1a0444);box-sizing: border-box;padding-top:70rpx;}
	.bigNum{font-size: 60rpx;font-weight: bold;color: #e1be69;}
	.boxTwo{margin-top: -150rpx;padding: 30rpx 4%;}
	.dayNIP .item{width: 66rpx;margin-right: 28rpx;}
	.dayNIP .item:nth-of-type(7){margin-right: 0;}
	.dayNIP .item .num{width: 66rpx;height: 66rpx;border-radius: 50%;background: #F5F5F5;margin-bottom: 10rpx;}
	
	/* 弹框 */
	.alertShow{width: 60%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 45;}
	.alertShow .closeBtn{bottom: auto;left: auto;top: 0rpx;right: 0;transform: inherit}
	.fudaiIcon{width: 376rpx; height: 381rpx; display: block;}
	.alertShow .num{padding: 0 20rpx;color: #ffff00; font-size: 52rpx;}
</style>
