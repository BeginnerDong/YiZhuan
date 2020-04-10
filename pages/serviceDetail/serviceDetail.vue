<template>
	<view>
		<view class="banner-box">
			<swiper style="width: 100%;height: 400rpx;" class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#757575">
				<block v-for="(item,index) in bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image style="width: 100%;height: 400rpx;" :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="mglr4 pdtb15">
			<view class="compayTit flex">
				<view class="text color2 fs15" style="font-weight: bold;">广州专业10年注册公司记账报税</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		
		<view class="f5H5"></view>
		<view class="mglr4 pdtb15">
			<view class="flex fs14 ftw"><image class="TitIcon" src="../../static/images/service-details-icon.png" mode=""></image><text>服务内容</text></view>
			<view class="msgInfor fs12">
				<view class="item">
					<text class="color6">服务区域：</text>
					<text>广东·广州市</text>
				</view>
				<view class="item flexY">
					<view class="color6" style="width: 21%;">代办项目：</view>
					<view style="width: 100%;">广告设计、制作。代理、发布、灯饰设计、展览展示服务、会议服务、图文设计制作、绿化养护、礼仪服务、动漫设计、计算机软硬件、园林古建筑工程专业施工、建筑装饰建设工程专项设计。</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="mglr4 pdtb15">
			<view class="flex fs14 ftw"><image class="TitIcon" src="../../static/images/service-details-icon3.png" mode=""></image><text>更多服务</text></view>
			<view class="msgInfor fs12">
				
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="mglr4 pdtb15">
			<view class="flex fs14 ftw"><image class="TitIcon" src="../../static/images/service-details-icon2.png" mode=""></image><text>执照资质</text></view>
			<view class="msgInfor fs12">
				<view class="item">
					<image class="img" src="../../static/images/service-details-img2.png"></image>
					<image class="img" src="../../static/images/service-details-img2.png"></image>
					<image class="img" src="../../static/images/service-details-img2.png"></image>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="buyPeople  mglr4">
			<view class="flexRowBetween">
				<view class="flex" style="width: 50%;">
					<view class="headImg flexCenter">
						<image src="../../static/images/service-details-img.png"></image>
					</view>
					<view style="margin-left: 20rpx;">发布人：张宇</view>
				</view>
				<view class="red">156789008908</view>
			</view>
		</view>
		<!-- 
		<view class="submitbtn fx-subBtn pdtb15 whiteBj boxShaow">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/companyDetail-orderMsg/companyDetail-orderMsg'}})">发起交易</button>
			<view class="agreeSel pdt5 fs12 color9 flex">
				<view class="selt mgr5" @click="agrren"><image class="icon" :src="num==true?'../../static/images/company-details-icon9.png':'../../static/images/company-details-icon8.png'" mode=""></image></view>
				<view @click="xuzhiShow">交易须知</view>
			</view>
		</view>
		
		<view class="xieyiAlert" v-show="is_xuzhiShow">
			<view class="infor">
				<view class="closeBtn" style="top: auto;bottom: 30rpx;"  @click="xuzhiShow">×</view>
				<view class="center fs14 pdb10">交易须知</view>
				<view class="cont fs12">
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
				]
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
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
