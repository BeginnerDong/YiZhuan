<template>
	<view>
		<button style="border-radius: 10rpx;width: 600rpx;margin: 300rpx auto 0 auto;background-color: #36b6ee;height: 80rpx;line-height: 80rpx;text-align: center;color: #fff;font-size:28rpx">
			绑定已有企业
		</button>
		<button @click="Router.navigateTo({route:{path:'/pages/service-IcanDo/service-IcanDo'}})" style="border:1px solid #36b6ee;border-radius: 10rpx;width: 600rpx;margin: 50rpx auto 0 auto;height: 80rpx;line-height: 80rpx;text-align: center;color: #36b6ee;font-size:28rpx">
			创建新企业
		</button>
		
		<input style="border:1px solid #36b6ee;border-radius: 10rpx;width: 600rpx;margin: 300rpx auto 0 auto;background-color: #fff;height: 80rpx;
		line-height: 80rpx;text-align: center;font-size:28rpx" placeholder="输入企业码">
			
		</input>
		<button style="border-radius: 10rpx;width: 600rpx;margin: 50rpx auto 0 auto;background-color: #36b6ee;height: 80rpx;line-height: 80rpx;text-align: center;color: #fff;font-size:28rpx">
			确定
		</button>
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
	page{
		background-color: #f5f5f5;
	}
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
