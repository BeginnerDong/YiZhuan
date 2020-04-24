<template>
	<view>
		
		<view class="mglr4">
			<view class="">
				<view class="item" style="border-bottom: 1px solid #f5f5f5" v-for="(item,index) in mainData" :key="index">
					<view class="flexRowBetween pdtb15">
						<view class="fs13 color2">{{index+1}}.{{item.title}}</view>
						<view @click="viewThis(index)"><image class="qimage" :src="viewIndex==index?'../../static/images/about-icon4.png':'../../static/images/about-icon3.png'" mode=""></image></view>
					</view>
					<view class="fs12 color6 pdb15" v-if="viewIndex==index">{{item.description}}</view>
				</view>
				
			</view>
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
				viewIndex:-1,
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
			
			viewThis(index){
				const self = this;
				if(index!=self.viewIndex){
					self.viewIndex = index
				}else{
					self.viewIndex = -1
				}
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
	.qimage{
		width: 20rpx;
		height:20rpx;
		
	}
</style>
