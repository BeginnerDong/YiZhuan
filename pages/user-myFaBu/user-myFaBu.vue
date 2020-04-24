<template>
	<view>
		<view class="orderNav pdlr4 flexRowBetween whiteBj">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">我要买</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">我要卖</view>
			<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">我能办</view>
		</view>
		
		<view class="mglr4">
			
			<view class="companyList pdtb15"  v-show="curr==1">
				<view class="item radius10 whiteBj" v-for="(item,index) in mainData" @click="Router.navigateTo({route:{path:'/pages/companyDetail/companyDetail'}})">
					<view class="no fs11 color9">公司编号：{{item.id}}</view>
					<view class="name fs15 pubColor center">{{item.title}}</view>
					<view class="text fs13 color2">成立时间:<span class="fs13 color6">{{item.establish_time}}</span></view>
					<view class="text fs13 color2">注册资本:<span class="fs13 color6">{{item.registered_capital}}万元</span></view>
					<view class="text fs13 color2">实缴资本:<span class="fs13 color6">{{item.paid_in_capital}}万元</span></view>
					<!-- <view class="flex tip">
						<view class="tip-item fs12 color6" style="background-color: #fff0f0;">空壳</view>
						<view class="tip-item fs12 color6" style="background-color: #f0f3ff;">买过保险</view>
						<view class="tip-item fs12 color6" style="background-color: #e4fff0;">小规模</view>
					</view> -->
					<view class="flexEnd">
						<view class="flex mgr25">
							<view class="mgr5"><image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon.png"></image></view>
							<view class="fs13 color6">编辑</view>
						</view>
						<view class="flex">
							<view class="mgr5"><image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon1.png"></image></view>
							<view class="fs13 color6">删除</view>
						</view>
					</view>
					<view class="type">
						<image class="icon" src="../../static/images/2.png" mode=""></image>
						<view class="text fs11 white">{{item.label&&item.label[item.menu_id]?item.label[item.menu_id].title:''}}</view>
					</view>
					<view class="button">
						<view class="underBtn flexCenter">
							<view class="midBtn flexCenter">
								<view class="upBtn flexCenter white">
									面议
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class=" buyList pdtb15"  v-show="curr==2">
				<view class="item radius10 whiteBj" v-for="(item,index) in mainData">
					<view class="title avoidOverflow3 fs14">{{item.title}}</view>
					<view class="flexRowBetween tip">
						<view class="flex">
							<view class="tip-item fs12 color6" style="background-color: #fff0f0;">空壳</view>
							<view class="tip-item fs12 color6" style="background-color: #f0f3ff;">买过保险</view>
							<view class="tip-item fs12 color6" style="background-color: #e4fff0;">小规模</view>
						</view>
						<view class="reward flex">
							<image src="../../static/images/qiugou-icon2.png"></image>
							<view class="fs10 colorffad">赚2金币</view>
						</view>
					</view>
					<view class="buyPeople flex">
						
						<view class="phone center">
							<button class="flexCenter">
								<image class="icon" src="../../static/images/qiugou-icon.png"></image>
								<view class="fs12 white">联系他</view>
							</button>
							
						</view>
						<view class="name center">
							<button class="flexCenter">
								<image class="icon" src="../../static/images/qiugou-icon1.png"></image>
								<view class="fs12 white">担保交易</view>
								
							</button>
							
						</view>
					</view>
					<view class="fs12 color9 mgt10">发布人位置：{{item.city}}</view>
					<view class="flexEnd">
						<view class="flex mgr25">
							<view class="mgr5"><image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon.png"></image></view>
							<view class="fs13 color6">编辑</view>
						</view>
						<view class="flex">
							<view class="mgr5"><image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon1.png"></image></view>
							<view class="fs13 color6">删除</view>
						</view>
					</view>
				</view>
			</view>
		
			<view class="serviceList pdtb15"  v-show="curr==3">
				<view class="item radius10 whiteBj" v-for="(item,index) in mainData" 
				@click="Router.navigateTo({route:{path:'/pages/serviceDetail/serviceDetail'}})">
					<view class="fs14 color2">西安纯粹云信息科技有限公司</view>
					<view class="flex tip">
						<view class="tip-item fs12 color6" style="background-color: #fff0f0;">空壳</view>
						<view class="tip-item fs12 color6" style="background-color: #f0f3ff;">买过保险</view>
						<view class="tip-item fs12 color6" style="background-color: #e4fff0;">小规模</view>
						<view class="tip-item fs12 color6" style="background-color: #fff0f0;">空壳</view>
						<view class="tip-item fs12 color6" style="background-color: #f0f3ff;">买过保险</view>
						<view class="tip-item fs12 color6" style="background-color: #e4fff0;">小规模</view>
						<view class="tip-item fs12 color6" style="background-color: #fff0f0;">空壳</view>
						<view class="tip-item fs12 color6" style="background-color: #f0f3ff;">买过保险</view>
						<view class="tip-item fs12 color6" style="background-color: #e4fff0;">小规模</view>
					</view>
					<view class="imgbox">
						<view class="lisThree">
							<image src="../../static/images/service-img.png" mode="aspectFill"></image>
						
						</view>
					</view>
					<view class="flex info">
						<view class="phone flex">
							<image src="../../static/images/service-icon.png">
							<view class="fs13 color6">15623234567</view>
						</view>
						<view  class="address flex">
							<image src="../../static/images/service-icon1.png">
							<view class="fs13 color6">广东·深圳市</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="flex mgr25">
							<view class="mgr5"><image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon.png"></image></view>
							<view class="fs13 color6">编辑</view>
						</view>
						<view class="flex">
							<view class="mgr5"><image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon1.png"></image></view>
							<view class="fs13 color6">删除</view>
						</view>
					</view>
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
				curr:1,
				companyDate:[{},{}],
				wantBuyDate:[{},{}],
				searchItem:{
					thirdapp_id:2
				},
				labelData:[],
				mainData:[],
				idArray:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData'], self);
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
			
			tel(){
				wx.makePhoneCall({
					phoneNumber: '15623455454',
				})
			},
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
					if(self.curr==1){
						self.searchItem.menu_id = ['in',self.idArray]
					}else if(self.curr==2){
						self.searchItem.menu_id = 8
					}else if(self.curr==3){
						self.searchItem.menu_id = 10
					};
					self.getMainData(true)
				}
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					parentid:9
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData,res.info.data)
						for (var i = 0; i < self.labelData.length; i++) {
							self.idArray.push(self.labelData[i].id)
						}
						self.searchItem.menu_id = ['in',self.idArray]
						self.getMainData()
					}
				};
				self.$apis.labelGet	(postData, callback);
			},
			
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					spu:{
						tableName:'SkuLabel',
						middleKey:'spu_item',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			

		},
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/buy.css";
	@import "../../assets/style/index.css";
	@import "../../assets/style/service.css";
	page{background: #F5F5F5;}
	.orderNav .tt{width: 33.3%;}
	button{
		background: none;
		line-height: 1.5;
		height:80rpx;
		padding: 10rpx 50rpx;
		width: 300rpx;
	}
	button::after{
		border: none;
	}
	.button-hover{
		color: #000000;
		background: none;
	}
</style>
