<template>
	<view>
		<view class="banner-box">
			<swiper style="width: 100%;height: 400rpx;" class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#757575">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image style="width: 100%;height: 400rpx;" :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="mx-3 py-3">
			<view class="compayTit d-flex a-center mb-2">
				<view class="text color2 font-32 font-weight">{{mainData.title?mainData.title:''}}</view>
			</view>
			<view class="font-24 mb-2 color9">公司编号：{{mainData.id?mainData.id:''}}</view>
			<view class="d-flex j-sb a-center">
				<view class="ll font-26">
					<view >出售价：<text class="font-36 red">面议</text></view>
					<view class="color9 mt-2 font-24">更新时间：<text>{{mainData.update_time}}</text></view>
				</view>
				<view class="reward d-flex a-center">
					<image src="../../static/images/qiugou-icon2.png"></image>
					<view class="font-24" style="color: #ffad42;">赚{{share}}金币</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		
		<view class="buyPeople d-flex a-center mx-3">
			
			<view class="phone text-center">
				<button class="d-flex j-center a-center" @click="conact">
					<image class="icon" src="../../static/images/qiugou-icon.png"></image>
					<view class="font-28 text-white">联系他</view>
				</button>
				
			</view>
			<view class="name text-center">
				<button class="d-flex j-center a-center">
					<image class="icon" src="../../static/images/qiugou-icon1.png"></image>
					<view class="font-28 text-white">担保交易</view>
					
				</button>
				
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		<view class="mx-3 py-3">
			<view class="d-flex a-center font-28 font-weight"><image class="TitIcon" src="../../static/images/the-company-details-icon.png" mode=""></image><text>基本信息</text></view>
			<view class="msgInfor font-26">
				<view class="d-flex j-sb a-center">
					<view class="item">
						<text class="color6">注册资本：</text>
						<text>{{mainData.registered_capital}}万人民币</text>
					</view>
					<view class="item">
						<text class="color6">实缴资本：</text>
						<text>{{mainData.paid_in_capital}}万人民币</text>
					</view>
				</view>
				
				<view class="item">
					<text class="color6">成立时间：</text>
					<text>{{mainData.establish_time}}</text>
				</view>
				<view class="item">
					<text class="color6">注册地址：</text>
					<text>{{mainData.province}} {{mainData.city}} {{mainData.country}}******</text>
				</view>
				<view class="item d-flex a-start">
					<view class="color6" style="width: 20%;">经营范围：</view>
					<view style="width:80%;">
						{{mainData.business_scope}}
					</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		
		<view class="" v-for="(item,index) in array" :key="index">
			<view class="mx-3 py-3">
				<view class="d-flex a-center font-28 font-weight"><image class="TitIcon" src="../../static/images/the-company-details-icon1.png" mode=""></image><text>税务信息</text></view>
				<view class="msgInfor font-26">
					<view class="d-flex a-center" style="flex-wrap: wrap;">
						<view class="item" v-for="(c_item,c_index) in item.data" :key="c_index">
							<text class="color6">{{c_item.title}}：</text>
							<text>小规模</text>
						</view>
					</view>
	
				</view>
			</view>
			<view class="f5H5"></view>
		</view>
		
		<view class="f5H5 "></view>
		<view class="mx-3 py-3">
			<view class="d-flex a-center font-28 font-weight"><image class="TitIcon" src="../../static/images/the-company-details-icon5.png" mode=""></image><text>执照资质</text></view>
			<view class="msgInfor font-26">
				<view class="item">
					<image class="img" v-for="item in mainData.bannerImg" :src="item.url"></image>
				</view>
			</view>
		</view>
		<!-- 
		<view class="submitbtn fx-subBtn py-3 bg-white shadow-sm">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/companyDetail-orderMsg/companyDetail-orderMsg'}})">发起交易</button>
			<view class="agreeSel mt-10 font-24 color9 d-flex a-center">
				<view class="selt mr-10" @click="agrren"><image class="icon" :src="num==true?'../../static/images/company-details-icon9.png':'../../static/images/company-details-icon8.png'" mode=""></image></view>
				<view @click="xuzhiShow">交易须知</view>
			</view>
		</view>
		
		<view class="xieyiAlert" v-show="is_xuzhiShow">
			<view class="infor">
				<view class="closeBtn" style="top: auto;bottom: 30rpx;"  @click="xuzhiShow">×</view>
				<view class="center font-28 pb-2">交易须知</view>
				<view class="cont font-24">
					<view>1、个人粉色发挥二十五后覅二姑夫规划局咖啡店是逻辑回归公交卡廊坊市开了个会；</view>
					<view>2、公交卡发生过了人生观估计快了发的是赶快来发过来好几个粉红色的洛凯股份股份深加工和发的是来了个；</view>
					<view>3、股份的股份上来看家给开发商老大哥就感觉分离的金盾股份了解公司和过分的话刚发的刚发的刚发的赛挂电话通过话的他回个话超凡大师；</view>
				</view>
			</view>
		</view> -->
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
				num:false,
				is_xuzhiShow:false,
				bannerImg:[
					{url:'../../static/images/service-details-img1.png'}
				],
				mainData:{},
				share:0,
				array:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.share = uni.getStorageSync('user_info').thirdApp.share;
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		methods: {
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:self.mainData.menu_id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0];
						self.getSkuLabelData()
					}
				};
				self.$apis.labelGet	(postData, callback);
			},
			
			getSkuLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:['in',self.labelData.spu_array]
				};
				postData.getAfter = {
					child:{
						tableName:'SkuLabel',
						middleKey:'id',
						key:'parentid',
						searchItem:{
							status:1
						},
						condition:'in',
						order:{
							listorder:'desc'
						}
					}
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						/* for (var i = 0; i < res.info.data.length; i++) {
							Things[i]
						} */
						for (var i = 0; i < res.info.data.length; i++) {
							if (self.array.length > 0) {
								var hasone = false;
								for (var j = 0; j < self.array.length; j++) {
									if (res.info.data[i].group == self.array[j].menu) {
										self.array[j].data.push(res.info.data[i]);
										hasone = true;
									};
								};
								if (!hasone) {
									self.array.push({
										menu: res.info.data[i].group,
										data: [res.info.data[i]],
						
									});
								};
							} else {
								self.array.push({
									menu: res.info.data[i].group,
									data: [res.info.data[i]],
								})
							};
						};
						console.log('array',self.array)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.skuLabelGet(postData, callback);
			},
			
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
			
			conact(){
				const self = this;
				uni.showModal({
					title:'提示',
					content:'联系他将花费您'+uni.getStorageSync('user_info').thirdApp.cost+'金币',
					success(res) {
						if(res.confirm){
							self.flowLogAdd()
						}
					}
				});
			},
			
			flowLogAdd() {
				const self = this;
				if(parseFloat(uni.getStorageSync('user_info').thirdApp.cost)>parseFloat(self.userInfoData.score)){
					uni.showModal({
						title:'提示',
						content:'您的金币不足',
						showCancel:false,
						success(res) {
							if(res.confirm){
								
							}
						}
					});
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					count: -parseFloat(uni.getStorageSync('user_info').thirdApp.cost),
					thirdapp_id:2,
					status:1,
					trade_info:'联系',
					type:3,
					account:1,
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						uni.makePhoneCall({
							phoneNumber:self.mainData.phone
						})
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:self.id
				};
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
						self.getLabelData()
					}
					
				};
				self.$apis.articleGet(postData, callback);
			},
			
			agrren(){
				const self = this;
				self.num = !self.num
			},
			xuzhiShow(){
				const self = this;
				self.is_xuzhiShow = !self.is_xuzhiShow
			},
			tel(){
				wx.makePhoneCall({
					phoneNumber: '15623455454',
				})
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/companyDetail.css";
	@import "../../assets/style/xieyiAlert.css";
	
	page{padding-bottom: 50rpx;}
	button{
		background: none;
		line-height: 1.5;
		height:80rpx;
		padding: 10rpx 50rpx;
		width: 330rpx;
	}
	button::after{
		border: none;
	}
	.button-hover{
		color: #000000;
		background: none;
	}
</style>
