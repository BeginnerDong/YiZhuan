<template>
	<view>
		
		<view class="px-3 bg-white py-3 topBox">
			<view class="address d-flex j-sb a-center font-26">
				<view class="d-flex a-center">
					<view>陕西省·西安市</view>
					<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				</view>
				<view class="d-flex a-center"  @click="Router.navigateTo({route:{path:'/pages/screen/screen?id=9'}})">
					<image class="icon1" src="../../static/images/home-icon0.png" mode=""></image>
					<view>筛选</view>
				</view>
			</view>
			<view class="companyType d-flex a-center">
				<view class="item color6 font-24" v-for="(item,index) in titleArray" :key="index">{{item}}
					<image src="../../static/images/1.png" @click="deleteChoose(index)"></image>
				</view>
			</view>
		</view>
		
		<view class="mx-3 companyList py-3">
			<view class="item rounded20 bg-white" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/companyDetail/companyDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="no font-22 color9">公司编号：{{item.id}}</view>
				<view class="name font-30 main-text-color text-center">{{item.title}}</view>
				<view class="text font-26 color2">成立时间:<span class="color6">{{item.establish_time}}</span></view>
				<view class="text font-26 color2">注册资本:<span class="color6">{{item.registered_capital}}万元</span></view>
				<view class="text font-26 color2">实缴资本:<span class="color6">{{item.paid_in_capital}}万元</span></view>
				<view class="d-flex a-center tip text-center font-24 color6">
					<view class="tip-item" v-for="(c_item,c_index) in item.spu">{{c_item.title}}</view>
				</view>
				<view class="type text-center">
					<image class="icon" src="../../static/images/2.png" mode=""></image>
					<view class="text font-22 text-white">{{item.label&&item.label[item.menu_id]?item.label[item.menu_id].title:''}}</view>
				</view>
				<view class="button text-white">
					<view class="underBtn d-flex j-center a-center">
						<view class="midBtn d-flex j-center a-center">
							<view class="upBtn d-flex j-center a-center text-center font-26">面议</view>
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
		<picker mode="selector" :range="labelData" @change="chooseType" range-key="title">
			<view class="fx-fabubtn" >
				<image class="icon" src="../../static/images/fabu.png" mode=""></image>
			</view>
		</picker>
		
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
				idArray:[],
				getBefore:{},
				itemArray:[],
				titleArray:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData','getUserInfoData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		onPullDownRefresh() {
			const self = this;
			console.log('start pulldown');
			self.idArray = [];
			self.labelData = [];
			self.titleArray = [];
			self.itemArray = [];
			self.mainData = [];
			self.getBefore = {};
			uni.setStorageSync('indexSearch',self.itemArray);
			uni.setStorageSync('indexTitle',self.titleArray);
			self.getLabelData()
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('indexSearch')&&uni.getStorageSync('indexSearch').length>0){
				self.titleArray = uni.getStorageSync('indexTitle');
				self.itemArray = uni.getStorageSync('indexSearch');
				self.getBefore = {
					article:{
						tableName:'SkuRelation',
						middleKey:'id',
						key:'relation_id',
						searchItem:{
							sku_item:['in',self.itemArray]
						},
						condition:'in'
					}
				};
				
			}else{
				self.getBefore = {}
			};
			self.getMainData(true)
		},
		
		methods: {
			
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
			
			deleteChoose(index){
				const self = this;
				var position = index;
				self.itemArray.splice(position, 1);
				self.titleArray.splice(position, 1);
				if(self.itemArray.length==0){
					self.getBefore = {}
				};
				self.getMainData(true)
				uni.setStorageSync('indexSearch',self.itemArray);
				uni.setStorageSync('indexTitle',self.titleArray);
			},
			
			
			chooseType(e){
				const self = this;
				console.log(e)
				self.Router.navigateTo({route:{path:'/pages/company-wantSell/company-wantSell?id='+self.labelData[e.detail.value].id}})
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
				if(JSON.stringify(self.getBefore)!='{}'){
					postData.getBefore = self.getBefore
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					 uni.stopPullDownRefresh();
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
	
	.tip-item:nth-child(n + 1) {
	   background-color: #fff0f0;
	 }
	 .tip-item:nth-child(2n + 1) {
	    background-color: #f0f3ff;
	 }
	 .tip-item:nth-child(3n + 1) {
			  background-color: #e4fff0;
	 }
</style>
