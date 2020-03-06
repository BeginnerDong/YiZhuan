<template>
	<view>
		
		<view class="mglr4">
			<view class="">
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
