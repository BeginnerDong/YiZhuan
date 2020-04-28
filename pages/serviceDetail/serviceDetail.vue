<template>
	<view>
		<view class="banner-box">
			<swiper style="width: 100%;height: 400rpx;" class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#757575">
				<block v-for="(item,index) in mainData.mainImg" :key="index">
					<swiper-item class="swiper-item">
						<image style="width: 100%;height: 400rpx;" :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="px-3 py-3 px-3 py-3">
			<view class="compayTit  d-flex a-center">
				<view class="text color2 font-30 font-weight">{{mainData.title}}</view>
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		
		
		<view class="px-3 py-3">
			<view class="d-flex a-center font-28 font-weight"><image class="TitIcon" src="../../static/images/service-details-icon.png" mode=""></image><text>服务内容</text></view>
			<view class="msgInfor font-26">
				<view class="item d-flex a-start pt-1">
					<text class="color6" style="width: 20%;">服务区域：</text>
					<text style="width: 80%;">{{mainData.province?mainData.province:''}}·{{mainData.city?mainData.city:''}}</text>
				</view>
				<view class="item d-flex a-start pt-1">
					<view class="color6" style="width: 20%;">代办项目：</view>
					<view style="width: 80%;">
						{{mainData.spu_content&&mainData.spu_content[0]?mainData.spu_content[0].content:''}}
					</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		<view class="px-3 py-3">
			<view class="d-flex a-center font-28 font-weight"><image class="TitIcon" src="../../static/images/service-details-icon3.png" mode=""></image><text>更多服务</text></view>
			<view class="msgInfor font-26">
				{{mainData.spu_content&&mainData.spu_content[0]?mainData.spu_content[0].content:''}}
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		<view class="px-3 py-3">
			<view class="d-flex a-center font-28 font-weight"><image class="TitIcon" src="../../static/images/service-details-icon2.png" mode=""></image><text>执照资质</text></view>
			<view class="msgInfor font-26">
				<view class="item">
					<image class="img" v-for="item in mainData.bannerImg" :src="item.url"></image>
					
				</view>
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		
		<view class="buyPeople  px-3">
			<view class="d-flex j-sb a-center ">
				<view class="d-flex a-center" style="width: 50%;">
					<view class="headImg d-flex a-center j-center">
						<image class="rounded-circle overflow-h" :src="mainData.User?mainData.User.headImgUrl:''"></image>
					</view>
					<view class="ml-2">发布人：{{mainData.User?mainData.User.nickname:''}}</view>
				</view>
				<view class="red">{{mainData.phone}}</view>
			</view>
		</view>
		<!-- 
		<view class="submitbtn fx-subBtn py-3 bg-white boxShaow">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/companyDetail-orderMsg/companyDetail-orderMsg'}})">发起交易</button>
			<view class="agreeSel mt-1 font-24 color9 d-flex a-center">
				<view class="selt mr-1" @click="agrren"><image class="icon" :src="num==true?'../../static/images/company-details-icon9.png':'../../static/images/company-details-icon8.png'" mode=""></image></view>
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
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					spu:{
						tableName:'SkuLabel',
						middleKey:'spu_item',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'in'
					},
					User:{
						token:uni.getStorageSync('user_token'),
						tableName:'User',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'in',
						info:['nickname','headImgUrl']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					self.$Utils.finishFunc('getMainData');
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
		height:50rpx;
		padding: 10rpx 50rpx;
	}
	button::after{
		border: none;
	}
	.button-hover{
		color: #000000;
		background: none;
	}
	
	.buyPeople .headImg{width: 25%;}
	.buyPeople .headImg image{width: 80rpx;height: 80rpx;}
</style>
