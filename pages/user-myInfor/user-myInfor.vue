<template>
	<view>

		<view class="myRowBetween fs13 mx-3">
			<view class="item d-flex j-sb a-center">
				<view class="ll color6">头像</view>
				<view class="rr">
					<view class="userPhoto border">
						<open-data type="userAvatarUrl"></open-data>
					</view>
				</view>
			</view>
			<view class="item d-flex j-sb a-center">
				<view class="ll flex color6">昵称</view>
				<view class="rr">
					<open-data type="userNickName"></open-data>
				</view>
			</view>
			<!-- <view class="item d-flex j-sb a-center" >
				<view class="ll flex color6">账号</view>
				<view class="rr"><input type="text" v-model="submitData.wechat" placeholder="请输入微信号" placeholder-class="placeholder"></view>
			</view> -->
			<view class="item d-flex j-sb a-center" v-if="mainData.phone&&mainData.phone!=''" @click="Router.navigateTo({route:{path:'/pages/user-phone/user-phone'}})">
				<view class="ll color6">绑定手机号</view>
				<view class="rr">
					{{mainData.phone}}
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image>
				</view>
			</view>

			<view class="item d-flex j-sb a-center" v-else>
				<view class="ll color6">绑定手机号</view>
				<button class="rr" open-type="getPhoneNumber" @getphonenumber="getPhoneNumber">
					点击绑定手机号
				</button>
			</view>
			<!-- <view class="item d-flex j-sb a-center" @click="Router.navigateTo({route:{path:'/pages/user-setUp-password/user-setUp-password'}})">
				<view class="ll flex color6">密码</view>
				<view class="rr flexEnd">
			
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image>
				</view>
			</view> -->
		</view>



	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils: this.$Utils,
				showView: false,
				score: '',
				wx_info: {},
				is_seltSexShow: false,
				is_show: false,
				mainData: {}
			}
		},

		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {

			getPhoneNumber(e) {
				const self = this;
				console.log('e', e);
				if (e.detail.errMsg == 'getPhoneNumber:fail user deny') {
					return
				};
				const postData = {
					appid: uni.getStorageSync('user_info').thirdApp.appid,
					tokenFuncName: 'getProjectToken',
					encryptedData: e.detail.encryptedData,
					iv: e.detail.iv
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.userInfoUpdate(res.info.phoneNumber)
					};
				}
				self.$apis.decryptWxInfo(postData, callback)
			},
			
			userInfoUpdate(phone) {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					phone:phone
				};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (data) => {
					if (data.solely_code == 100000) {
						self.Router.navigateTo({route:{path:'/pages/user-phone/user-phone'}})
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			

			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},



		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";

	.userPhoto {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
		overflow: hidden;
	}

	.userPhoto image {
		width: 100%;
		height: 100%;
		border-radius: 50%;
		display: block;
	}

	.seltSexShow {
		width: 80%;
		position: fixed;
		top: 50%;
		left: 50%;
		z-index: 50;
		transform: translate(-50%, -50%);
	}

	.seltSexShow .item {
		padding: 30rpx 4%;
		border-bottom: 1px solid #eee;
	}

	.seltSexShow .item:last-child {
		border-bottom: 0;
	}
	
	button{
		background: none;
	}
	button::after{
		border: none;
	}
	.button-hover{
		color: #000000;
		background: none;
	}
</style>
