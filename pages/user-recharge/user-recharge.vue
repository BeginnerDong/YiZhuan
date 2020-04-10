<template>
	<view>
		<view class="mglr4">
			<view class="">
				<view class="myExtendTop white" style="position: relative;display: flex;">
					<image src="../../static/images/img.png" style="position: absolute;width: 100%;height:300rpx"></image>
					<view class="" style="z-index: 999;margin-left: 50rpx;">
						<view class="fs15 mgt30">金币：</view>
						<view class="money mgt20">500</view>
					</view>
				</view>
				<view class="fs14 color2 mgb25" style="font-weight: bold;">充值金额</view>
				<viwe>
					<input style="border-bottom: 1px solid #848484;width: 100%;color: red;font-weight: bold;" class="fs15" value="100"/>
				</viwe>
				<view class="gold  flexRowBetween">
					<view class="item fs16" v-for="(item,index) in goldDate" :key="index">
						{{item}}元
					</view>
					<view class="item on">
						自定义
					</view>
				</view>
			</view>
		</view>
		<button style="border-radius: 10rpx;width: 600rpx;margin: 200rpx auto 0 auto;background-color: #36b6ee;height: 80rpx;line-height: 80rpx;text-align: center;color: #fff;font-size:28rpx">支付</button>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				goldDate:[100,200,500,800,1000]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
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
	.myExtendTop{width: 100%;margin-top: 30rpx;height: 370rpx;}
	.myExtendTop .money{font-size: 80rpx; line-height:88rpx;}
	.gold{margin-top: 20rpx;flex-wrap: wrap;}
	.gold .item{width:210rpx;height: 100rpx;line-height: 100rpx;border: 1px solid #c0c0c0;text-align: center;margin-top: 30rpx;border-radius: 10rpx;}
	.gold .on{border: 1px solid #36b6ee;color: #36b6ee;}
</style>
