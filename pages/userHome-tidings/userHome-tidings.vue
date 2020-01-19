<template>
	<view>
		
		<view class="pdlr4">
			<view class="dialogBox" v-for="(item,index) in mainData">
				<view class="item left" v-if="item.user_no!=me">
					<view class="time center fs13 color9">{{item.create_time}}</view>
					<view class="infor">
						<image class="Photo" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" >
						<view class="text">{{item.content}}</view>
					</view>
				</view>
				<view class="item right" v-if="item.user_no==me">
					<view class="time center fs13 color9">{{item.create_time}}</view>
					<view class="infor">
						<view class="text">{{item.content}}</view>
						<image class="Photo" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" >
					</view>
				</view>
			</view>
		</view>
		
		<view class="fix-talkIput flexRowBetween whiteBj fs13">
			<view class="input">
				<input type="text" v-model="submitData.content" placeholder="" placeholder-class="placeholder">
			</view>
			<view class="flexEnd">
				<button class="send fs13" type="button" @click="$Utils.stopMultiClick(addChat)">发送</button>
			</view>
			
		</view>
		
		
	</view>
</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				is_show:false,
				mainData:[],
				submitData:{
					name:'',
					mainImg:[],
					content:'',
					relation_id:''
				},
				me:'',
				searchItem:{
					thirdapp_id:2,
					
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.id = options.id;
			self.searchItem.relation_id = self.id;
			self.paginate.last_page = true;
			self.submitData.name=uni.getStorageSync('user_info').info.name;
			self.submitData.mainImg=uni.getStorageSync('user_info').info.mainImg;
			self.submitData.relation_id=self.id;
			self.me = uni.getStorageSync('user_info').user_no;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				delete self.paginate.last_page;
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		onPullDownRefresh() {
			const self = this;
			console.log('refresh');
			delete self.paginate.last_page;
			//self.paginate = self.page;
			console.log('self.paginate',self.paginate)
			self.paginate.currentPage--;
			console.log('self.paginate',self.paginate)
			self.getMainData(true);
			
			console.log('self.paginate',self.paginate)
		},
		
		methods: {
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				
				postData.tokenFuncName = 'getUserToken';
				postData.order = {
					create_time:'asc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					uni.stopPullDownRefresh();
					if(postData.paginate.last_page){
						self.paginate.currentPage = res.info.page;
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.chatGet(postData, callback);
			},
			
			addChat() {
				const self = this;
				var nowTime = Date.parse(new Date())/1000;
				uni.setStorageSync('canClick', false);
				if(self.submitData.content==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('内容不能为空', 'none', 1000);
					return
				};
				const postData = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.tokenFuncName = 'getUserToken';
				/* postData.saveAfter = [{
					tableName: 'Relation',
					FuncName: 'update',
					searchItem: {
						id: self.id
					},
					data: {
						update_time: nowTime,
					}
				}]; */
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.submitData.content = '';
						self.getMainData(true)
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.chatAdd(postData, callback);
			},
		},
	}
</script>


<style>
	@import "../../assets/style/tidings.css";
	page{padding-bottom: 130rpx;background: #F5F5F5;}
	
	
</style>

