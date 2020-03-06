<template>
	<view>
		
		<view class="pdlr4 whiteBj pdt15 pdb15 topBox">
			<view class="address flexRowBetween">
				<view class="flexRowBetween">
					<view class="fs12">陕西省·西安市</view>
					<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				</view>
				<view class="flexRowBetween"  @click="Router.navigateTo({route:{path:'/pages/screen/screen'}})">
					<image class="icon1" src="../../static/images/home-icon0.png" mode=""></image>
					<view class="fs12">筛选</view>
				</view>
				
			</view>
			<view class="companyType flex">
				<view class="item color6 fs11">空壳公司
					<image src="../../static/images/1.png"></image>
				</view>
				<view class="item color6 fs11">买过保险
					<image src="../../static/images/1.png"></image>
				</view>
			</view>
		</view>
		
		
		
		<view class="mglr4  companyList pdtb15">
			<view class="item radius10 whiteBj" v-for="(item,index) in mainData" @click="Router.navigateTo({route:{path:'/pages/companyDetail/companyDetail'}})">
				<view class="no fs11 color9">公司编号：432423423</view>
				<view class="name fs15 pubColor center">深圳市***教育公司</view>
				<view class="text fs13 color2">成立时间:<span class="fs13 color6">2018.02.23</span></view>
				<view class="text fs13 color2">注册资本:<span class="fs13 color6">100万人民币</span></view>
				<view class="text fs13 color2">实缴资本:<span class="fs13 color6">12万人民币</span></view>
				<view class="flex tip">
					<view class="tip-item fs12 color6" style="background-color: #fff0f0;">空壳</view>
					<view class="tip-item fs12 color6" style="background-color: #f0f3ff;">买过保险</view>
					<view class="tip-item fs12 color6" style="background-color: #e4fff0;">小规模</view>
				</view>
				<view class="type">
					<image class="icon" src="../../static/images/2.png" mode=""></image>
					<view class="text fs11 white">物流类</view>
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
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">找公司</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/buy/buy'}})" >
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
					<image src="../../static/images/nabar4.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		<view class="fx-fabubtn" @click="Router.navigateTo({route:{path:'/pages/login/login'}})">
			<image class="icon" src="../../static/images/fabu.png" mode=""></image>
		</view>
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
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					}
					console.log('self.userInfoData', self.userInfoData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					title:'首页轮播',
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData = res.info.data[0]
					}
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/index.css";
	@import "../../assets/style/top.css";
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	
</style>
