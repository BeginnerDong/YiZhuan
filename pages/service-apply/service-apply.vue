<template>
	<view>
		
		<view class="editLine">
			<view class="item d-flex j-sb a-center">
				<view class="ll">企业名称：</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入企业名称" placeholder-class="placeholder"/>
				</view>
			</view>
			<view class="item d-flex j-sb a-center">
				<view class="ll">联系方式：</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入联系方式" placeholder-class="placeholder"/>
				</view>
			</view>
			<view class="item d-flex j-sb a-center">
				<view class="ll">登录密码：</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入登录密码" placeholder-class="placeholder"/>
				</view>
			</view>
			<view class="item d-flex j-sb a-center">
				<view class="ll">验证码：</view>
				<view class="rr d-flex j-end a-center">
					<input style="width: 50%;" type="text" value="" placeholder="请输入验证码" placeholder-class="placeholder"/>
					<view class="main-text-color ml-3">获取验证码</view>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/service-IcanDo/service-IcanDo'}})">确定</button>
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
				num:false
			}
		},
		
		onLoad() {
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
	@import "../../assets/style/editInfor.css";
	
	.editLine .item{padding: 30rpx 4%;}
	.editLine .item .ll{}
	.editLine .item .rr{font-size: 26rpx;}
	.editLine .item input{text-align: right;}
</style>
