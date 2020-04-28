<template>
	<view>
		<view class="userInfo pubBj d-flex j-sb a-center">
			<view class="ll d-flex a-center">
				<view class="headImg rounded-circle overflow-h border"><open-data type="userAvatarUrl"></open-data></view>
				<view class="ml-2">
					<view class="name font-30 text-white"><open-data type="userNickName"></open-data></view>
				</view>
			</view>
		</view>
		<button class="d-flex j-center a-center px-1" 
		style="width: 120rpx;height: 50rpx;position: absolute;top: 10%;right: 0%;background-color: #5bcdff;border-radius: 30rpx 0 0 30rpx;">
			<image src="../../static/images/icon.png" style="width:30rpx;height: 30rpx;"></image>
			<view class="font-24 text-white ml-1">分享</view>
		</button>
		<view class="gold d-flex j-sb a-center mx-3 px-3 bg-white rounded20" style="height: 130rpx; margin-top: -60rpx;">
			<view class="ll d-flex a-center">
				<image style="width: 50rpx;height: 50rpx;" src="../../static/images/icon1.png"></image>
				<view class="font-26 color2 ml-2">金币：</view>
				<view class="font-34 font-weight" style="color: #fdc72f;">
				{{userInfoData.score?userInfoData.score:'0.00'}}</view>
			</view>
			<view><button @click="Router.navigateTo({route:{path:'/pages/user-recharge/user-recharge'}})" style="width: 150rpx;height: 60rpx;line-height: 60rpx;color: #fff;border-radius: 50rpx; background-color: #36b6ee;font-size: 28rpx;">充值</button></view>
		</view>
		<view class="mx-3 my-3">
			<view class="menu d-flex j-sb a-center text-center font-26 color2 rounded20 bg-white">
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myInfor/user-myInfor'}})">
					<view class="d-flex j-center a-center">
						<image src="../../static/images/about-icon0.png"></image>
					</view>
					<view class="mt-3">基本信息</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myGold/user-myGold'}})">
					<view class="d-flex j-center a-center">
						<image src="../../static/images/about-icon.png"></image>
					</view>
					<view class="mt-3">我的金币</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myFaBu/user-myFaBu'}})">
					<view  class="d-flex j-center a-center">
						<image src="../../static/images/about-icon1.png"></image>
					</view>
					<view class="mt-3">我的发布</view>
				</view>
				<view class="item" @click="viewOradd">
					<view  class="d-flex j-center a-center">
						<image src="../../static/images/about-icon2.png"></image>
					</view>
					<view class="fs13 color2" style="text-align: center;margin-top: 30rpx;">我的企业</view>
				</view>
			</view>
			<view class="pt-3">
				<view class="boxShaow question bg-white px-4 rounded20">
					<view class="d-flex j-sb a-center title pt-4 pb-1">
						<view class="font-30 color2">常见问题</view>
						<view class="d-flex a-center" @click="Router.navigateTo({route:{path:'/pages/question/question'}})">
							<view class="font-24 color9 mr-2">查看全部</view>
							<view><image src="../../static/images/about-icon3.png"></image></view>
						</view>
					</view>
					
					<view class="item border-bottom" v-for="(item,index) in mainData" :key="index">
						<view class="d-flex j-sb a-center py-3">
							<view class="font-26 color2">{{index+1}}.{{item.title}}</view>
							<view @click="viewThis(index)"><image :src="viewIndex==index?'../../static/images/about-icon4.png':'../../static/images/about-icon3.png'" mode="">
							</image></view>
						</view>
						
						<view class="font-24 pb-3 color6" v-if="viewIndex==index">{{item.description}}</view>
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
	/* .button-hover{
		color: #000000;
		background: none;
	} */
	.userInfo{
		width: 100%;
		height: 320rpx;
		padding: 0 4%;
		box-sizing: border-box;
		background-color: #36b6ee;
	}
	.userInfo .headImg{
		width: 120rpx;
		height: 120rpx;
	}
	.userInfo .headImg image{
		width: 100%;
		height: 100%;
		display: block;
	}
	.menu{
		width: 100%;
		height: 200rpx;
	}
	.menu .item{flex: 1;}
	.menu .item image{
		width: 50rpx;
		height: 50rpx;
	}
	.question .item:last-child{border-bottom: 0;}
	.question image{
		width: 14rpx;
		height:23rpx;
		
	}
</style>
