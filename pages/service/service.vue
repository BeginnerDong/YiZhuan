<template>
	<view>
		
		<view class="topBox px-3 bg-white py-3">
			<view class="address  d-flex j-sb a-center">
				<view class="d-flex j-sb a-center">
					<view>陕西省·西安市</view>
					<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				</view>
				<view class="d-flex j-sb a-center"  @click="Router.navigateTo({route:{path:'/pages/screen/screen'}})">
					<image class="icon1" src="../../static/images/home-icon0.png" mode=""></image>
					<view>筛选</view>
				</view>
				
			</view>
			<view class="companyType d-flex a-center">
				<view class="item color6 font-22" v-for="(item,index) in titleArray" :key="index">{{item}}
					<image src="../../static/images/1.png" @click="deleteChoose(index)"></image>
				</view>
			</view>
		</view>
		
		
		
		<view class="mx-3  serviceList py-3">
			<view class="item rounded20 bg-white mb-3 p-3 bg-white position-relative" v-for="(item,index) in mainData" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/serviceDetail/serviceDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="font-28 color2">{{item.title}}</view>
				<view class="d-flex a-center tip pt-2 flex-wrap">
					<view class="tip-item font-24 color6" v-for="(c_item,c_index) in item.spu">{{c_item.title}}</view>
				</view>
				<view class="imgbox d-flex a-center">
					<view class="lisThree">
						<image v-for="c_item in item.mainImg" :src="c_item.url" mode="aspectFill"></image>
					
					</view>
				</view>
				<view class="d-flex a-center info mt-2 font-26 color6">
					<view class="phone d-flex a-center mr-5">
						<image src="../../static/images/service-icon.png">
						<view class="ml-1">{{item.phone}}</view>
					</view>
					<view  class="address d-flex a-center">
						<image src="../../static/images/service-icon1.png">
						<view class="ml-1">{{item.province}}·{{item.city}}</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item"  @click="Router.redirectTo({route:{path:'/pages/index/index'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">找公司</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/buy/buy'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">求购</view>
			</view>
			<view class="navbar_item">
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">服务大厅</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar4.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		<view class="fx-fabubtn" @click="Router.navigateTo({route:{path:'/pages/service-IcanDo/service-IcanDo?id=10'}})">
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
				mainData:[],
				searchItem:{
					thirdapp_id:2
				},
				getBefore:{},
				itemArray:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self);
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
			self.titleArray = [];
			self.itemArray = [];
			self.mainData = [];
			self.getBefore = {};
			uni.setStorageSync('serviceSearch',self.itemArray);
			uni.setStorageSync('serviceTitle',self.titleArray);
			self.getLabelData()
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('serviceSearch')&&uni.getStorageSync('serviceSearch').length>0){
				self.titleArray = uni.getStorageSync('serviceTitle');
				self.itemArray = uni.getStorageSync('serviceSearch');
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
			
			deleteChoose(index){
				const self = this;
				var position = index;
				self.itemArray.splice(position, 1);
				self.titleArray.splice(position, 1);
				if(self.itemArray.length==0){
					self.getBefore = {}
				};
				self.getMainData(true)
				uni.setStorageSync('serviceSearch',self.itemArray);
				uni.setStorageSync('serviceTitle',self.titleArray);
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
					menu_id:['in',10]
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
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/top.css";
	@import "../../assets/style/service.css";
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
