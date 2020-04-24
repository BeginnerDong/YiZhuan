<template>
	<view>
		<view class="">
			<view class="myExtendTop white flexCenter center">
				<view class="">
					<view class="money">{{userInfoData.score?userInfoData.score:'0.00'}}</view>
					<view class="fs14">总金币(个)</view>
				</view>
			</view>
			
			<view class="myRowBetween pdlr4">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="ll">
						<view>{{item.trade_info}}</view>
						<view class="fs12 color6 pdt15">{{item.create_time}}</view>
					</view>
					<view class="rr red fs18">{{item.count}}</view>
				</view>
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
				searchItem:{
					thirdapp_id:2,
					type:3
				},
				mainData:[],
				userInfoData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id:2
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData(isNew) {
				var self = this;
				if (isNew) {
					self.messageData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
			
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myExtendTop{width: 100%;height: 320rpx; background: #0FA2E5;}
	.myExtendTop .money{font-size: 80rpx; line-height:88rpx;}
	
	.myRowBetween .item .rr{font-size: 30rpx;}
</style>
