<template>
	<view>
		<view v-if="!isEdit">
			<view class="center" style="margin-top: 110rpx;">
				<image style="width:260rpx;height: 340rpx;" src="../../static/images/icon2.png"></image>
				<view class="fs14 color2" style="margin-top: 70rpx;">绑定手机号：{{mainData.phone}}</view>
			</view>
			
			
			
			<view class="submitbtn" style="margin-top: 100rpx;">
				<button class="btn" type="submint" @click="goEdit">更换手机号</button>
			</view>
		</view>
		
		
		<view v-if="isEdit">
			<view class="editLine">
				<view class="item flexRowBetween">
					
					<view class="rr">
						<input type="number" maxlength="11" v-model="submitData.phone" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					
					<view class="rr flexRowBetween" style="width: 100%;">
						<view style="width: 50%;">
							<input type="number" value="" placeholder="请输入验证码" placeholder-class="placeholder" />
						</view>
						<view class="pubColor mgl10">获取验证码</view>
					</view>
				</view>
			</view>
			<view class="submitbtn" style="margin-top: 100rpx;">
				<button class="btn" type="submint" @click="Utils.stopMultiClick(userInfoUpdate)">完成</button>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils: this.$Utils,
				isEdit:false,
				mainData:{},
				submitData:{
					phone:''
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {
			
			userInfoUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.phone==''){
					self.$Utils.showToast('请输入手机号', 'none', 1000);
					uni.setStorageSync('canClick', true);
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					phone:self.submitData.phone
				};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (data) => {
					if (data.solely_code == 100000) {
						self.$Utils.showToast('修改成功', 'none', 1000)
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

			goEdit(){
				const self = this;
				self.isEdit = !self.isEdit
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
