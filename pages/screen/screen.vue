<template>
	<view>
		<view class="screenList font-26 pt-3 mx-3">
			<view class="item d-flex j-sb a-start" v-for="(item,index) in mainData" :key="index" v-if="item.style==1">
				<view class="ll">{{item.title}}：</view>
				<view class="rr d-flex a-start">
					<view class="btn" :class="item.chooseStatus==-1?'on':''" @click="choose(index,-1)">不限</view>
					<view class="btn" v-for="(c_item,c_index) in item.children" :key="c_index" :class="Utils.inArray(c_item.id,itemArray)>-1?'on':''"
					 @click="choose(index,c_index)">{{c_item.title}}</view>
				</view>
			</view>
			<!-- <view class="item flexRowBetween">
				<view class="ll">注册年限：</view>
				<view class="rr flex">
					<view class="btn" v-for="(item,index) in registerReg" :key="index" :class="currRegisterReg == index?'on':''" @click="seltRegisterReg(index)">{{item}}</view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">注册资本：</view>
				<view class="rr flex">
					<view class="btn" v-for="(item,index) in registerCapital" :key="index" :class="currRegisterCapital == index?'on':''" @click="seltRegisterCapital(index)">{{item}}</view>
				</view>
			</view> -->

		</view>

		<view class="submitbtn" style="margin-top: 100rpx;">
			<button class="btn" type="button" @click="save">确定</button>
		</view>

	</view>

</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				showView: false,
				Utils: this.$Utils,
				wx_info: {},
				is_show: false,
				currCategory: 0,
				category: ['不限', '空壳公司', '实体公司'],
				currProvince: 0,
				province: ['不限', '北京', '天津', '河北', '陕西', '山西', '内蒙古', '辽宁', '吉林', '上海', '江苏', '内蒙古', '浙江', '安徽', '福建', '四川', '湖南',
					'湖北', '河南'
				],
				currHotLable: 0,
				hotLable: ['不限', '免费转', '买过社保', '银行流水大', '政府/国企背景', '外资背景', '国家局名称'],
				currHotCompany: 0,
				hotCompany: ['不限', '资质类公司', '商标类公司', '网络资产', '专利/制作权', '车牌类公司', '金融/投资', '香港公司'],
				currRegisterReg: 0,
				registerReg: ['不限', '1年以下', '1年及以上', '2年及以上', '3年及以上', '5年及以上', '10年及以上'],
				currRegisterCapital: 0,
				registerCapital: ['不限', '10万以下', '10万及以上', '50万及以上', '100万及以上', '1000万及以上'],
				currPayTaxes: 0,
				payTaxes: ['不限', '小规模', '一般纳税人'],
				mainData: [],
				itemArray: [],
				titleArray: []
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},

		onShow() {
			const self = this;

		},

		methods: {

			save() {
				const self = this;
				if (self.id == 9) {
					uni.setStorageSync('indexSearch', self.itemArray)
					uni.setStorageSync('indexTitle', self.titleArray)
				} else if (self.id == 8) {
					uni.setStorageSync('buySearch', self.itemArray)
					uni.setStorageSync('buyTitle', self.titleArray)
				} else if (self.id == 10) {
					uni.setStorageSync('serviceSearch', self.itemArray)
					uni.setStorageSync('serviceTitle', self.titleArray)
				};
				uni.navigateBack({
					delta: 1
				})
			},

			choose(index, c_index) {
				const self = this;

				if (c_index == -1) {
					for (var i = 0; i < self.mainData[index].children.length; i++) {
						if (self.itemArray.indexOf(self.mainData[index].children[i].id) >= 0) {

							self.itemArray.splice(self.itemArray.indexOf(self.mainData[index].children[i].id), 1)
						}
					}
				} else {
					var id = self.mainData[index].children[c_index].id;
					var isChooseAll = true
					var position = self.itemArray.indexOf(id);
					if (position >= 0) {
						self.itemArray.splice(position, 1);
						self.titleArray.splice(position, 1);
					} else {
						self.itemArray.push(id);
						self.titleArray.push(self.mainData[index].children[c_index].title);
					};
					for (var i = 0; i < self.mainData[index].children.length; i++) {
						if (self.itemArray.indexOf(self.mainData[index].children[i].id) < 0) {
							isChooseAll = false;
							self.mainData[index].chooseStatus = 1
						}
					};
					console.log('isChooseAll', isChooseAll)
					if (isChooseAll) {
						self.mainData[index].chooseStatus = -1
						for (var i = 0; i < self.mainData[index].children.length; i++) {
							console.log('22', self.mainData[index].children[i].id)
							self.itemArray.splice(self.itemArray.indexOf(self.mainData[index].children[i].id), 1);
						};
					}
				}
				console.log('self.itemArray', self.itemArray)
			},


			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id
				};
				const callback = (res) => {
					if (res.info.length > 0) {
						self.mainData.push.apply(self.mainData, res.info);
						if (self.id == 9 && uni.getStorageSync('indexSearch')) {
							self.itemArray = uni.getStorageSync('indexSearch');
							self.titleArray = uni.getStorageSync('indexTitle');
						} else if (self.id == 8 && uni.getStorageSync('buySearch')) {
							self.itemArray = uni.getStorageSync('buySearch');
							self.titleArray = uni.getStorageSync('buyTitle');
						} else if (self.id == 10 && uni.getStorageSync('serviceSearch')) {
							self.itemArray = uni.getStorageSync('serviceSearch');
							self.titleArray = uni.getStorageSync('serviceTitle');
						};
						console.log(self.itemArray);
						console.log(self.titleArray);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].chooseStatus = -1
						}
						for (var i = 0; i < self.mainData.length; i++) {
							if (self.mainData[i].children) {
								for (var j = 0; j < self.mainData[i].children.length; j++) {
									if (self.itemArray.indexOf(self.mainData[i].children[j].id) >= 0) {
										self.mainData[i].chooseStatus = 1
									}
								}
							}
						}
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.getTree(postData, callback);
			},
		}
	};
</script>

<style>
	page {
		padding-bottom: 60rpx;
	}

	.screenList .item {
		align-items: flex-start;
		margin-bottom: 10rpx;
	}

	.screenList .ll {
		width: 27%;
		box-sizing: border-box;
	}

	.screenList .rr {
		width: 73%;
		flex-wrap: wrap;
	}

	.screenList .rr .btn {
		padding: 0 16rpx;
		line-height: 40rpx;
		border-radius: 20rpx;
		background: #eee;
		margin: 0 30rpx 30rpx 0;
	}

	.screenList .rr .btn.on {
		background: #0FA2E5;
		color: #fff;
	}
</style>
