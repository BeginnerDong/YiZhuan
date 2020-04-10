<template>
	<view>
		
		<view class="center" style="margin-top: 110rpx;">
			<image style="width:260rpx;height: 340rpx;" src="../../static/images/icon2.png"></image>
			<view class="fs14 color2" style="margin-top: 70rpx;">绑定手机号：41748748487</view>
		</view>
		
		
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<button class="btn" type="submint">更换手机号</button>
		</view>
		<view class="editLine">
			<view class="item flexRowBetween">
				
				<view class="rr">
					<input type="number" value="" placeholder="请输入手机号" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				
				<view class="rr">
					<view style="width: 50%;">
						<input type="number" value="" placeholder="请输入验证码" placeholder-class="placeholder" />
					</view>
					<view class="pubColor mgl10">获取验证码</view>
				</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top: 100rpx;">
			<button class="btn" type="submint">完成</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils: this.$Utils,
				index: 0,
				is_show: false,
				type: '',
				mode: '',
				submitData: {
					o_password: '',
					n_password: '',
					passwordCopy: ''
				}
			}
		},
		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
		},

		methods: {



			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);

				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
					if (self.submitData.o_password != uni.getStorageSync('user_info').password) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('原密码错误', 'none');
						return
					};
					if (self.submitData.n_password != self.submitData.passwordCopy) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('两次输入密码不一致', 'none');
						return
					};
					self.passwordUpdate();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},

			passwordUpdate() {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					password:self.submitData.n_password
				};
				const callback = (data) => {
					if (data.solely_code == 100000) {
						self.$Utils.showToast('修改成功,请重新登陆', 'none');
						setTimeout(function() {
							uni.removeStorageSync('user_token');
							uni.removeStorageSync('user_info');
							uni.reLaunch({
								url: '/pages/index/index'
							});
						}, 800);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
				};
				self.$apis.userUpdate(postData, callback);
			},

		},
	};
</script>


<style>
	/* .paswd .item{} */
	.paswd .item .ll{width: 20%;}
	.paswd .item .rr{width: 80%;border-bottom: 1px solid #eee;padding: 30rpx 0;}
	.placeholder{font-size: 26rpx;}
	.editLine .item{ align-items: flex-start; line-height: 48rpx;padding: 30rpx 0;border-bottom: 1px solid #eee;margin: 0 auto;width:80%}
	.editLine .item .ll{width: 25%; }
	.editLine .item .rr{width: 75%;}
	.editLine .L-input{width: 85%;}
	.editLine .item input{width: 100%; line-height: 48rpx;height: 48rpx;display: block;box-sizing: border-box;font-size: 26rpx; color: #666;}
	.editLine .item textarea{width: 100%;height:200rpx; padding: 20rpx; box-sizing: border-box;border: 1px solid #eee; font-size: 24rpx;display: block;}
</style>
