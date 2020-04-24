<template>
	<view>
		<view class="userInfo pubBj flexRowBetween">
			<view class="ll flex">
				<view class="headImg">
					<view class="headImg"  style="overflow: hidden;border-radius: 50%;"><open-data type="userAvatarUrl"></open-data></view>
				</view>
				<view style="margin-left: 20rpx;">
					<view class="name fs15"><open-data type="userNickName"></open-data></view>
					
				</view>
			</view>
			
		</view>
		<button class="flexCenter" 
		style="width: 140rpx;height: 50rpx;position: absolute;top: 10%;right: 0%;background-color: #5bcdff;border-radius: 25rpx 0 0 25rpx;">
			<image src="../../static/images/icon.png" style="width:30rpx;height: 30rpx;"></image>
			<view class="fs12 white" style="margin-left: 10rpx;">分享</view>
		</button>
		<view class="gold flexRowBetween" 
		style="height: 130rpx;width: 84%;margin: 0 auto;background-color: #fff;position: absolute;top: 21%;left: 4%;padding: 0 4%;border-radius: 10rpx;">
			<view class="ll flex">
				<image style="width: 50rpx;height: 50rpx;" src="../../static/images/icon1.png"></image>
				<view class="fs13 color2" style="margin-left: 20rpx;">金币：<span style="color: #fdc72f;font-size: 34rpx;font-weight: bold;">
				{{userInfoData.score?userInfoData.score:'0.00'}}</span></view>
			</view>
			<view><button @click="Router.navigateTo({route:{path:'/pages/user-recharge/user-recharge'}})" style="width: 150rpx;height: 60rpx;line-height: 60rpx;color: #fff;border-radius: 50rpx;" class="fs14  pubBj">充值</button></view>
		</view>
		<view class="" style="margin: 100rpx 4%;">
			<view class="menu flexRowBetween" style="border-radius: 10rpx;">
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myInfor/user-myInfor'}})">
					<view class="flexCenter">
						<image src="../../static/images/about-icon0.png"></image>
					</view>
					<view class="fs13 color2" style="text-align: center;margin-top: 30rpx;">基本信息</view>
				</view>
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
				<view class="item" @click="viewOradd">
					<view  class="flexCenter">
						<image src="../../static/images/about-icon2.png"></image>
					</view>
					<view class="fs13 color2" style="text-align: center;margin-top: 30rpx;">我的企业</view>
				</view>
			</view>
			<view class="pdt15">
				<view class="boxShaow question" style="border-radius: 10rpx;">
					<view class="flexRowBetween title">
						<view class="fs15 color2">常见问题</view>
						<view class="flex" @click="Router.navigateTo({route:{path:'/pages/question/question'}})">
							<view class="fs12 color9 mgr5">查看全部</view>
							<view><image src="../../static/images/about-icon3.png"></image></view>
						</view>
					</view>
					
					<view class="item" v-for="(item,index) in mainData" :key="index">
						<view class="flexRowBetween pdtb15">
							<view class="fs13 color2">{{index+1}}.{{item.title}}</view>
							<view @click="viewThis(index)"><image :src="viewIndex==index?'../../static/images/about-icon4.png':'../../static/images/about-icon3.png'" mode="">
							</image></view>
						</view>
						
						<view class="fs12 color6 pdb15" v-if="viewIndex==index">{{item.description}}</view>
					</view>
					
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
				mainData:[],
				searchItem:{
					thirdapp_id:2
				},
				viewIndex:-1,
				userInfoData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			viewThis(index){
				const self = this;
				if(index!=self.viewIndex){
					self.viewIndex = index
				}else{
					self.viewIndex = -1
				}
			},
			
			viewOradd(){
				const self = this;
				if(self.userInfoData.company>0){
					self.Router.navigateTo({route:{path:'/pages/serviceDetail/serviceDetail?id='+self.userInfoData.company}})
				}else{
					self.Router.navigateTo({route:{path:'/pages/user-company/user-company'}})
				}
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.paginate={
					count: 0,
					currentPage:1,
					pagesize:3,
					is_page:true,
				},
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['常见问题']],
						},
						condition:'in'
					}
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					};
					console.log(self.mainData.content)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	
	page{padding-bottom: 140rpx;background-color: #f5f5f5;}
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
		background-color: #fff;
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
		background-color: #fff;
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
