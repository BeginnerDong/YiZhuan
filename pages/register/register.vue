<template>
	<view>
		<view class="loginEdit">
			<view class="font-weight mt-5 pt-5" style="font-size:52rpx;">注册账号</view>
		</view>
		
		<view style="margin-top: 50px;">
			<view class="loginEdit">
				<view class="item d-flex a-center">
					
					<view class="rr">
						<input type="text" value="" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item d-flex a-center">
					
					<view class="rr" style="width: 76%;">
						<input type="text" value="" placeholder="请输入验证码" placeholder-class="placeholder" />
					</view>
					<view class="main-text-color">获取验证码</view>
				</view>
				
			</view>
			
			<view class="submitbtn" style="margin-top: 80rpx;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/login/login'}})">注册</button>
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
				is_show:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>

.loginEdit{padding: 0 10%;}
.loginEdit .item{padding:40rpx 0rpx 0 0rpx;border-bottom: 1px solid #eee;line-height: 80rpx;box-sizing: border-box;}
.loginEdit .item .ll{width: 20%;font-size: 30rpx;}
.loginEdit .item .icon{width: 44rpx; height: 44rpx;}
.loginEdit .item .rr{width: 80%;font-size: 26rpx;}
.loginEdit .item .rr input{width: 100%;height: 60rpx; display: block;font-size: 24rpx;}
.loginEdit .item .rr .placeholder{font-size: 26rpx;}
</style>

