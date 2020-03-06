<template>
	<view>
		<view class="userInfo pubBj flexRowBetween">
			<view class="ll flex">
				<view class="headImg">
					<image src="../../static/images/about-img.png"></image>
				</view>
				<view style="margin-left: 20rpx;">
					<view class="name fs15">多啦a梦</view>
					<view class="jinbi fs12" style="margin-top: 10rpx;">金币：31</view>
				</view>
			</view>
			<view class="chongzhi fs14 pubColor">充值</view>
		</view>
		<view class="menu flexRowBetween">
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myGold/user-myGold'}})">
				<view class="flexCenter">
					<image src="../../static/images/about-icon.png"></image>
				</view>
				<view class="fs13 color2" style="text-align: center;margin-top: 30rpx;">我的金币</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myFaBu/user-myFaBu'}})">
				<view  class="flexCenter">
					<image src="../../static/images/about-icon1.png"></image>
				</view>
				<view class="fs13 color2" style="text-align: center;margin-top: 30rpx;">我的发布</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/service-IcanDo/service-IcanDo'}})">
				<view  class="flexCenter">
					<image src="../../static/images/about-icon2.png"></image>
				</view>
				<view class="fs13 color2" style="text-align: center;margin-top: 30rpx;">我的企业</view>
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="mglr4 pdt30">
			<view class="boxShaow question">
				<view class="flexRowBetween title">
					<view class="fs15 color2">常见问题</view>
					<view class="flex" @click="Router.navigateTo({route:{path:'/pages/question/question'}})">
						<view class="fs12 color9 mgr5">查看全部</view>
						<view><image src="../../static/images/about-icon3.png"></image></view>
					</view>
				</view>
				
				<view class="item">
					<view class="flexRowBetween pdtb15">
						<view class="fs13 color2">1.如何得到金币？</view>
						<view><image src="../../static/images/about-icon4.png" mode=""></image></view>
					</view>
					
					<view class="fs12 color6 pdb15">付水电费水电费水电费第三方对方水电费水电费</view>
				</view>
				<view class="item flexRowBetween pdtb15">
					<view  class="fs13 color2">2.如何得到金币？</view>
					<view><image src="../../static/images/about-icon3.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween pdtb15">
					<view class="fs13 color2">3.如何得到金币？</view>
					<view><image src="../../static/images/about-icon3.png" mode=""></image></view>
				</view>
			</view>
		</view>
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">找公司</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/buy/buy'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">求购</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/service/service'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">服务大厅</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar4-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				productData:[{},{},{},{},{},{}],
				sliderData:{},
				mainData:[{},{}],
				searchItem:{
					thirdapp_id:2
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getSliderData','getMainData','getUserInfoData'], self);
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
			
		
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	
	page{padding-bottom: 140rpx}
	.userInfo{
		width: 92%;
		height: 320rpx;
		padding: 0 4%;
	}
	.userInfo .headImg{
		width: 120rpx;
		height: 120rpx;
	}
	.userInfo .name{
		color: #fff;
	}
	.userInfo .jinbi{
		color: #666;
		background-color: #fff;
		width: 140rpx;
		height: 40rpx;
		line-height: 40rpx;
		text-align: center;
		border-radius:10rpx
	}
	.userInfo .chongzhi{
		
		background-color: #fff;
		width: 120rpx;
		height: 50rpx;
		line-height: 50rpx;
		text-align: center;
		border-radius:20rpx
	}
	.userInfo .headImg image{
		width: 100%;
		height: 100%;
	}
	.menu{
		width: 100%;
		height: 200rpx;
	}
	.menu .item{
		width: 33.3%;
		
	}
	.menu .item image{
		width: 50rpx;
		height: 50rpx;
	}
	.question{
		width: 92%;
	
		padding:0 4%
	}
	.question .title{
		padding-top: 40rpx;
	}
	.question .item{
		width: 100%;
		/* height:100rpx;
		line-height:100rpx; */
		border-bottom: 1px solid #f5f5f5;
	}
	.question image{
		width: 20rpx;
		height:20rpx;
		
	}
</style>
