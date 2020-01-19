<template>
	<view>
		<view class="">
			<view class="signInHead center">
				<view class="white fs13 mgb15">NIP值</view>
				<view class="bigNum">{{userInfoData.score}}</view>
			</view>
			
			<view class="boxTwo boxShaow mglr4 whiteBj radius10 mgb20">
				<view class="flexRowBetween mgb20">
					<view class="fs13">{{userInfoData.logToday&&userInfoData.logToday.length==0?'今日':'明日'}}签到可获得<text class="red">{{willSignCount}}NIP</text></view>
					<view class="radsBtnH50 fs10"  v-if="userInfoData.logToday&&userInfoData.logToday.length==0" 
					 @click="Utils.stopMultiClick(submit)">立即签到</view>
					 <view class="radsBtnH50 fs10"  v-if="userInfoData.logToday&&userInfoData.logToday.length>0">已签到</view>
				</view>
				<view class="dayNIP flex fs12">
					<view class="item flexColumn" v-for="(item,index) in dayNIPData" :key="index">
						<view class="num flexCenter">+{{item.num}}</view>
						<view class="color6">{{item.day}}天</view>
					</view>
				</view>
			</view>
			
			<view class="sign-wrap">
				<view class="date-wrap">
					<view class="cur-date">{{curYear}}年{{curMonth+1}}月</view>
					<view class="title-item-box item-box flexRowBetween" >
						<view class="item"
							v-for="(item, index) in ['一','二','三','四','五','六','日']" 
							:key="index">{{item}}
						</view>
					</view>
					<view class="date-item-box item-box flex">
						<view class="item date-item" :class="item.count?'active':''" v-for="(item, index) in dateData" :key="index">
							<view class="dian ">{{item.sDay}}</view>
							<view class="num red fs12"><span v-if="item.count">+{{item.count}}</span></view>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<view class="black-bj" v-if="is_show"></view>
		<view class="alertShow" v-if="is_alertShow">
			<view class="closeBtn" @click="closeMsg()">×</view>
			<view class="flexCenter"><image class="fudaiIcon" src="../../static/images/sign-in-img.png" mode=""></image></view>
			<view class="white mgt25 center fs13">恭喜您获得<span class="num ftw">{{willSignCount}}</span>NIP</view>
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
				Utils:this.$Utils,
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				dayNIPData:[
					{num:'',day:'1',value:'one',week:1},
					{num:'',day:'2',value:'two',week:2},
					{num:'',day:'3',value:'three',week:3},
					{num:'',day:'4',value:'four',week:4},
					{num:'',day:'5',value:'five',week:5},
					{num:'',day:'6',value:'six',week:6},
					{num:'',day:'7',value:'seven',week:0}
				],
				is_alertShow:false,
				year: 2020,
				month: 1,
				date: 3,
				type:'today' ,//today-今天 pre-今天之前 next-今天之后
				curDate:'',
				dateArray:31,
				dateData:[],
				curYear:'',
				curMonth:'',
				chooseDay:'',
				logData:[],
				userInfoData:{},
				willSignCount:'',
				signDay:'',
				
			}
		},
		onLoad() {
			const self = this;
			console.log(uni.getStorageSync('user_info').thirdApp.custom_rule)
			var obj = uni.getStorageSync('user_info').thirdApp.custom_rule;
			for(let i in obj){
				for (var j = 0; j < self.dayNIPData.length; j++) {
					if(i==self.dayNIPData[j].value){
						self.dayNIPData[j].num = obj[i]
					}
				}
			};
			self.$Utils.loadAll(['getLogData'], self);
		},
		
		methods: {
			
			closeMsg(){
				const self = this;
				self.is_show = false;
				self.is_alertShow = false;
				self.getLogData()
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				self.addLog()
			},
			
			addLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 3,
					title: '签到成功',
					behavior:self.willSignCount,
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				postData.saveAfter = [{
					tableName: 'UserInfo',
					FuncName: 'update',
					searchItem: {
						user_no: self.userInfoData.user_no
					},
					data: {
						signin_day: self.signDay,
					}
				},
				{
					tableName: 'FlowLog',
					FuncName: 'add',
					data: {
						type: 3,
						trade_info:'签到',
						count:self.willSignCount,
						user_no:uni.getStorageSync('user_info').user_no,
						thirdapp_id:2,
						account:1
					}
				}];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.alertShow()
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					log:{
						tableName:'Log',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							type:3,
							create_time:['between',[dayStart-86400,dayStart]]
						},
						condition:'='
					},
					logToday:{
						tableName:'Log',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							type:3,
							create_time:['between',[dayStart,dayStart+86400]]
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					};
					
					self.countToday();
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			countToday(){
				const self = this;
				var now = new Date();
				var day = now.getDay();
				console.log('day',day);
				if(day==1){
					self.willSignCount = self.dayNIPData[0].num;
					self.signDay = 1;
				}else{
					if(self.userInfoData.log.length==0){
						self.willSignCount = self.dayNIPData[0].num
						self.signDay = 1;
					}else{
						for (var i = 0; i < self.dayNIPData.length; i++) {
							if(parseInt(self.dayNIPData[i].day)==self.userInfoData.signin_day+1){
								self.willSignCount = self.dayNIPData[i].num
							}
						}
						self.signDay = self.userInfoData.signin_day+1;
					}
				}
				self.$Utils.finishFunc('getLogData');
			},
			
			initNum(num){
				const self = this;
				if(num<10){
					return '0'+num
				}else{
					return num
				}
			},
			
			calenderInit() {
				const self = this;
				var curDate = new Date();
				self.curMonth = curDate.getMonth();
				self.curYear = curDate.getFullYear();
				self.curDay = curDate.getDate();
				/* self.chooseDay = self.initNum(self.curYear)+'-'+self.initNum(self.curMonth+1)+'-'+ self.initNum(self.curDay);
				console.log('self.chooseDay',self.chooseDay);
				self.chooseDay = self.chooseDay.replace(/\.|\-/g, '/')
				self.submitData.day_time = Date.parse(new Date(self.chooseDay+' 00:00:00'))/1000;
				console.log('self.submitData',self.submitData) */
				self.refreshPageData(self.curYear, self.curMonth, self.curDay);
			},
			
			//刷新全部数据
			refreshPageData(year, month, day) {
				const self = this;
			
				self.dateData = [];
				self.getOffset(self.curYear, self.curMonth);
				self.monthArray = [new Date(self.curYear, self.curMonth, 1).getTime(), new Date(self.curYear, self.curMonth + 1, 1)
					.getTime()
				];
			
				var offset = self.getOffset(self.curYear, self.curMonth);
				for (var i = 0; i < offset; ++i) {
					self.dateData.push({
						isEmpty: true
					});
				};
				var dayCount = self.getDayCount(self.curYear, self.curMonth);
				for (var i = 0; i < dayCount; ++i) {
					if (self.todayDay == i + 1) {
						self.dateData.push({
							sFull:self.initNum(self.curYear)+'-'+self.initNum(self.curMonth+1)+'-'+ self.initNum(i + 1),
							sDay: i + 1,
							isToday: true
						});
					} else {
						self.dateData.push({
							sFull:self.initNum(self.curYear)+'-'+self.initNum(self.curMonth+1)+'-'+ self.initNum(i + 1),
							sDay: i + 1
						});
					};
				};
				console.log('self.dateData', self.dateData)
				
				self.getUserInfoData()
				if(self.logData.length>0){
					for (var i = 0; i < self.dateData.length; i++) {
						for (var j = 0; j < self.logData.length; j++) {
							if(self.dateData[i].sFull==self.logData[j].create_time.substr(0,10)){
								self.dateData[i].count = self.logData[j].behavior
							}
						}
						
					}
				}
			},
			
			//获取此月第一天相对视图显示的偏移
			getOffset(curYear, curMonth) {
				const self = this;
				var offset = new Date(curYear, curMonth, 1).getDay();
				offset = offset == 0 ? 6 : offset - 1;
				//注意这个转换，Date对象的getDay函数返回返回值是 0（周日） 到 6（周六） 
				console.log('offset', offset)
				return offset;
			},
			isLeapYear(year) {
				if (year % 400 == 0 || (year % 4 == 0 && year % 100 != 0))
					return 1
				else
					return 0
			},
			
			getDayCount(year, month) {
				var DAY_OF_MONTH = [
					[31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
					[31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
				];
				return DAY_OF_MONTH[this.isLeapYear(year)][month];
			},
			
			getLogData() {
				const self = this;
				var data = new Date(); //本月
				data.setDate(1);
				data.setHours(0);
				data.setSeconds(0);
				data.setMinutes(0);
				var monthStart = parseInt(data.getTime() / 1000);
				var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				var nowTime = Date.parse(new Date()) / 1000;
				const postData = {
					searchItem:{
						type:3,
						create_time:['between',[monthStart,nowTime]],
						user_no:uni.getStorageSync('user_info').user_no
					}
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.logData.push.apply(self.logData,res.info.data)
					}
					self.calenderInit()
					self.$Utils.finishFunc('getLogData');
				};
				self.$apis.logGet(postData, callback);
			},
			
			alertShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_alertShow = !self.is_alertShow;
			},
			
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
	
	/* 日期 */
	.date-wrap{padding: 2% 0;border-radius: 16upx;background: #fff;box-sizing: border-box;text-align: center;}
	.cur-date{font-size: 30upx; margin-bottom: 30upx;}
	.item-box{color: #666;font-size: 28upx;}
	.item{width:14.2%; margin: 10upx 0;}
	.item .dian{width:44rpx;height:44upx ;line-height:44upx;border-radius: 100upx;margin: 0 auto;}
	.item .num{line-height:36rpx;font-size: 24upx;height: 36rpx;}
	.item.disabled .dian{color: #999;}
	.item.active .dian{background:#222;color: #fff;}
	.title-item-box{background: #F5F5F5;padding: 0 2%; margin-bottom: 30upx;}
	.date-item-box{padding: 0 2%;font-size: 24upx; flex-wrap: wrap;}
	.date-item.on .dian{background: #222; color: #fff;}
</style>
