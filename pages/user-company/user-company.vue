<template>
	<view>
		<view v-if="!isJoin">
			<button @click="join" style="border-radius: 10rpx;width: 600rpx;margin: 300rpx auto 0 auto;background-color: #36b6ee;height: 80rpx;line-height: 80rpx;text-align: center;color: #fff;font-size:28rpx">
				绑定已有企业
			</button>
			<button @click="Router.navigateTo({route:{path:'/pages/service-IcanDo/service-IcanDo?id=10'}})" style="border:1px solid #36b6ee;border-radius: 10rpx;width: 600rpx;margin: 50rpx auto 0 auto;height: 80rpx;line-height: 80rpx;text-align: center;color: #36b6ee;font-size:28rpx">
				创建新企业
			</button>
		</view>
		
		<view v-if="isJoin">
			<input style="border:1px solid #36b6ee;border-radius: 10rpx;width: 600rpx;margin: 300rpx auto 0 auto;background-color: #fff;height: 80rpx;
			line-height: 80rpx;text-align: center;font-size:28rpx" v-model="submitData.id" placeholder="输入企业码">
				
			</input>
			<button @click="Utils.stopMultiClick(userInfoUpdate)" style="border-radius: 10rpx;width: 600rpx;margin: 50rpx auto 0 auto;background-color: #36b6ee;height: 80rpx;line-height: 80rpx;text-align: center;color: #fff;font-size:28rpx">
				确定
			</button>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				isJoin:false,
				submitData:{id:''}
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			join(){
				const self = this;
				self.isJoin = !isJoin
			},
			
			userInfoUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.id==''){
					self.$Utils.showToast('请输入手机号', 'none', 1000);
					uni.setStorageSync('canClick', true);
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					company:self.submitData.id
				};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (data) => {
					if (data.solely_code == 100000) {
						self.$Utils.showToast('加入成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
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
