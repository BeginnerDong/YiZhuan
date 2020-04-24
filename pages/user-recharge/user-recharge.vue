<template>
	<view>
		<view class="mglr4">
			<view class="">
				<view class="myExtendTop white" style="position: relative;display: flex;">
					<image src="../../static/images/img.png" style="position: absolute;width: 100%;height:300rpx"></image>
					<view class="" style="z-index: 999;margin-left: 50rpx;">
						<view class="fs15 mgt30">金币：</view>
						<view class="money mgt20">{{mainData.score}}</view>
					</view>
				</view>
				<view class="fs14 color2 mgb25" style="font-weight: bold;">充值金额</view>
				<viwe v-if="curr==5">
					<input v-model="price" style="border-bottom: 1px solid #848484;width: 100%;color: red;font-weight: bold;" class="fs15" value="100"/>
				</viwe>
				<view class="gold  flexRowBetween">
					<view class="item fs16" :class="curr==index?'on':''" @click="changeCurr(index)"
					v-for="(item,index) in goldDate" :key="index">
						{{item}}元
					</view>
					<view class="item" :class="curr==5?'on':''"@click="changeCurr(5)">
						自定义
					</view>
				</view>
			</view>
		</view>
		<button @click="addOrder()" style="border-radius: 10rpx;width: 600rpx;margin: 200rpx auto 0 auto;background-color: #36b6ee;height: 80rpx;line-height: 80rpx;text-align: center;color: #fff;font-size:28rpx">
			支付</button>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				goldDate:[100,200,500,800,1000],
				curr:-1,
				price:0,
				orderId:'',
				mainData:{},
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
			pay() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				self.price =  parseFloat(self.price)
				const postData = {};	
				postData.wxPay = {
					price:self.price.toFixed(2)
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							type:3,
							count:self.price,
							trade_info:'充值',
							account:1,
							thirdapp_id:2,
							user_no:uni.getStorageSync('user_info').user_no
						},
					},
				];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('支付成功', 'none');
									setTimeout(function() {
										uni.navigateBack({
											delta: 1
										});
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast('支付失败', 'none');
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {						
							self.$Utils.showToast('支付成功', 'none');
							setTimeout(function() {
								uni.navigateBack({
									delta: 1
								});
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none');
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			addOrder() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				/* if(self.orderId!=''){
					self.pay();
					return
				}; */
				if(self.curr<0){
					self.$Utils.showToast('请选择充值金额', 'none');
					return
				};
				if(self.price<=0){
					self.$Utils.showToast('请输入充值金额', 'none');
					return
				};
				const postData = {};	
				/* postData.wxPay = {
					price:parseFloat(self.submitData.price).toFixed(2)
				}; */
				postData.tokenFuncName = 'getProjectToken',
				postData.data = {
					price:parseFloat(0.01).toFixed(2),
					level:2
				}
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					}else{
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.addVirtualOrder(postData, callback);
			},
			
			changeCurr(index){
				const self = this;
				
				if(self.curr!=index){
					self.curr = index;
					if(self.curr<5){
						self.price = parseFloat(self.goldDate[self.curr]);
					}else{
						self.price = 0
					}
				};
				
				console.log('self.price',self.price)
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	button{
		background: none;
		line-height: 1.5;
	}
	button::after{
		border: none;
	}
	.button-hover{
		color: #000000;
		background: none;
	}
	.myExtendTop{width: 100%;margin-top: 30rpx;height: 370rpx;}
	.myExtendTop .money{font-size: 80rpx; line-height:88rpx;}
	.gold{margin-top: 20rpx;flex-wrap: wrap;}
	.gold .item{width:210rpx;height: 100rpx;line-height: 100rpx;border: 1px solid #c0c0c0;text-align: center;margin-top: 30rpx;border-radius: 10rpx;}
	.gold .on{border: 1px solid #36b6ee;color: #36b6ee;}
</style>
