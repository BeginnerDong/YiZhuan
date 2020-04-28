<template>
	<view>
		
		<view class="topBox px-3 bg-white py-3">
			<view class="address  d-flex j-sb a-center" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<view class="d-flex j-sb a-center font-26">
					<view>陕西省·西安市</view>
					<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				</view>
				<view class="d-flex j-sb a-center">
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
		
		
		
		<view class="mx-3 py-3 buyList">
			<view class="item rounded20 bg-white mb-3 p-3 position-relative" v-for="(item,index) in mainData">
				<view class="title avoidOverflow3 font-28">{{item.title}}</view>
				<view class="d-flex j-sb a-center tip py-3">
					<view class="d-flex a-center flex-wrap font-24 color6" style="width: 60%;">
						<view class="tip-item" style="background-color: #fff0f0;">空壳</view>
						<view class="tip-item" style="background-color: #f0f3ff;">买过保险</view>
						<view class="tip-item " style="background-color: #e4fff0;">小规模</view>
					</view>
					
					<view class="reward d-flex a-center">
						<image src="../../static/images/qiugou-icon2.png"></image>
						<view class="font-22 colorffad ml-1">赚2金币</view>
					</view>
				</view>
				<view class="buyPeople d-flex j-sb a-center border-top pt-3">
					
					<view class="phone text-center">
						<button class="d-flex a-center j-center">
							<image class="icon mr-1" src="../../static/images/qiugou-icon.png"></image>
							<view class="font-28 text-white">联系他</view>
						</button>
					</view>
					<view class="name text-center">
						<button class="d-flex a-center j-center">
							<image class="icon mr-1" src="../../static/images/qiugou-icon1.png"></image>
							<view class="font-28 text-white">担保交易</view>
						</button>
					</view>
				</view>
				<view class="font-24 color9 mt-2">发布人位置：{{item.city}}</view>
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
			<view class="navbar_item">
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">求购</view>
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
		
		<view class="fx-fabubtn" @click="Router.navigateTo({route:{path:'/pages/wantBuy-buy/wantBuy-buy?id=8'}})">
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
			uni.setStorageSync('buySearch',self.itemArray);
			uni.setStorageSync('buyTitle',self.titleArray);
			self.getLabelData()
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('buySearch')&&uni.getStorageSync('buySearch').length>0){
				self.titleArray = uni.getStorageSync('buyTitle');
				self.itemArray = uni.getStorageSync('buySearch');
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
				uni.setStorageSync('buySearch',self.itemArray);
				uni.setStorageSync('buyTitle',self.titleArray);
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
					menu_id:['in',8]
				};
				/* postData.getAfter = {
					spu:{
						tableName:'SkuLabel',
						middleKey:'spu_item',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'in'
					}
				}; */
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
	@import "../../assets/style/buy.css";
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	button{
		background: none;
		line-height: 1.5;
		height:80rpx;
		padding: 10rpx 50rpx;
		width: 300rpx;
	}
	.colorffad{color: #ffad42;}
	
</style>
