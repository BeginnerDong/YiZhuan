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
				<view class="no fs11 color9">公司编号：{{item.id}}</view>
				<view class="name fs15 pubColor center">{{item.title}}</view>
				<view class="text fs13 color2">成立时间:<span class="fs13 color6">{{item.description}}</span></view>
				<view class="text fs13 color2">注册资本:<span class="fs13 color6">{{item.small_title}}</span></view>
				<view class="text fs13 color2">实缴资本:<span class="fs13 color6">{{item.passage1}}</span></view>
				<view class="flex tip">
					<view class="tip-item fs12 color6" style="background-color: #fff0f0;">空壳</view>
					<view class="tip-item fs12 color6" style="background-color: #f0f3ff;">买过保险</view>
					<view class="tip-item fs12 color6" style="background-color: #e4fff0;">小规模</view>
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
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					menu_id:9
				},
				labelData:[],
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					menu_id:['in',self.idArray]
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
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/index.css";
	@import "../../assets/style/top.css";
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	
</style>
