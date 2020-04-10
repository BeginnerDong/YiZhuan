<template>
	<view>
		<view class="loginEdit">
			<view style="font-size:26px;font-weight: 700;margin-top: 50px;">账号登录</view>
		</view>
		
		
		<view style="margin-top: 50px;">
			
			<view class="loginEdit" v-show="curr==1">
				<view class="item flex">
					
					<view class="rr">
						<input type="text" value="" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					
					<view class="rr" style="width: 76%;">
						<input type="text" value="" placeholder="请输入验证码" placeholder-class="placeholder" />
					</view>
					<view class="pubColor">获取验证码</view>
				</view>
				
			</view>
			
			<view class="loginEdit" v-show="curr==2">
				<view class="item flex">
					
					<view class="rr">
						<input type="text" value="" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">

					<view class="rr">
						<input type="text" value="" placeholder="请输入密码" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 80rpx;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/company-wantSell/company-wantSell'}})">登录</button>
			</view>
			<view class="loginEdit">
				<view class="flexRowBetween pdt10">
					<view class="color9 fs13" @click="changeCurr">密码登录</view>
					<view class="pubColor fs13" @click="Router.navigateTo({route:{path:'/pages/register/register'}})">没有账号立即注册</view>
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
				curr:1
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
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
@import "../../assets/style/orderNav.css";	
.orderNav .tt{width: 50%;}
.orderNav .tt.on::after{ width: 200rpx;}

.loginEdit{padding: 0 10%;}
.loginEdit .item{padding:40rpx 0 0 0;border-bottom: 1px solid #eee;line-height: 60rpx;box-sizing: border-box;}
.loginEdit .item .ll{width: 20%;font-size: 30rpx;}
.loginEdit .item .icon{width: 44rpx; height: 44rpx;}
.loginEdit .item .rr{width: 80%;font-size: 26rpx;}
.loginEdit .item .rr input{width: 100%;height: 60rpx; display: block;font-size: 24rpx;}
.loginEdit .item .rr .placeholder{font-size: 26rpx;}
</style>

