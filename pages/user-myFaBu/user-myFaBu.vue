<template>
	<view>
		<view class="orderNav pdlr4 d-flex j-sb a-center bg-white">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">我要买</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">我要卖</view>
			<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">我能办</view>
		</view>

		<view class="mx-3">

			<view class="companyList py-3" v-show="curr==1">
				<view class="item rounded20 bg-white" v-for="(item,index) in mainData">
					<view :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/companyDetail/companyDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="no font-24 color9">公司编号：{{item.id}}</view>
						<view class="name font-30 main-text-color text-center">{{item.title}}</view>
						<view class="text font-26 color2">成立时间:<span class="color6">{{item.establish_time}}</span></view>
						<view class="text font-26 color2">注册资本:<span class="color6">{{item.registered_capital}}万元</span></view>
						<view class="text font-26 color2">实缴资本:<span class="color6">{{item.paid_in_capital}}万元</span></view>
					</view>
					<view class="d-flex j-end a-center">
						<view class="d-flex a-center mr-5" :data-art_id="item.id"  :data-id="item.menu_id"
						@click="Router.navigateTo({route:{path:'/pages/company-wantSell/company-wantSell?id='+$event.currentTarget.dataset.id+'&art_id'+event.currentTarget.dataset.art_id}})">
							<view class="mr-1">
								<image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon.png"></image>
							</view>
							<view class="font-26 color6">编辑</view>
						</view>
						<view class="d-flex a-center" @click="deleteThis(index)">
							<view class="mr-1">
								<image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon1.png"></image>
							</view>
							<view class="font-26 color6">删除</view>
						</view>
					</view>
					<view class="type text-center">
						<image class="icon" src="../../static/images/2.png" mode=""></image>
						<view class="text font-24 text-white">{{item.label&&item.label[item.menu_id]?item.label[item.menu_id].title:''}}</view>
					</view>
					<view class="button">
						<view class="underBtn d-flex a-center j-center">
							<view class="midBtn d-flex a-center j-center">
								<view class="upBtn d-flex a-center j-center text-white">
									面议
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class=" buyList py-3" v-show="curr==2">
				<view class="item rounded20 bg-white mb-3 p-3 position-relative" v-for="(item,index) in mainData">
					<view class="title avoidOverflow3 font-28">{{item.title}}</view>
					<view class="d-flex j-sb a-center tip py-3">
						<view class="d-flex a-center">
							<view class="d-flex a-center flex-wrap font-24 color6" v-for="(c_item,c_index) in item.spu">{{item.title}}</view>
						</view>
						<view class="reward d-flex a-center ">
							<image src="../../static/images/qiugou-icon2.png"></image>
							<view class="font-22 colorffad  ml-1">赚{{share}}金币</view>
						</view>
					</view>
					<view class="buyPeople d-flex j-sb a-center border-top pt-3">
						<view class="phone center">
							<button class="d-flex j-center a-center" @click="conact(index)">
								<image class="icon  mr-1" src="../../static/images/qiugou-icon.png"></image>
								<view class="font-28 text-white">联系他</view>
							</button>
						</view>
						<view class="name center">
							<button class="d-flex j-center a-center">
								<image class="icon  mr-1" src="../../static/images/qiugou-icon1.png"></image>
								<view class="font-28 text-white">担保交易</view>
							</button>
						</view>
					</view>
					<view class="font-24 color9 mt-2">发布人位置：{{item.city}}</view>
					<view class="d-flex j-end a-center">
						<view class="d-flex a-center mr-5">
							<view class="mr-1">
								<image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon.png"></image>
							</view>
							<view class="font-26 color6">编辑</view>
						</view>
						<view class="d-flex a-center" @click="deleteThis(index)">
							<view class="mr-1">
								<image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon1.png"></image>
							</view>
							<view class="font-26 color6">删除</view>
						</view>
					</view>
				</view>
			</view>

			<view class="serviceList py-3" v-show="curr==3">
				<view class="item rounded20 bg-white mb-3 p-3 bg-white position-relative" v-for="(item,index) in mainData" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/serviceDetail/serviceDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="font-28 color2">{{item.title}}</view>
					<view class="d-flex a-center tip pt-2 flex-wrap">
						<view class="tip-item font-24 color6" v-for="(c_item,c_index) in item.spu">{{c_item.title}}</view>
					</view>
					<view class="imgbox d-flex a-center">
						<view class="lisThree">
							<image v-for="c_item in item.mainImg" :src="c_item.url" mode="aspectFill"></image>
						
						</view>
					</view>
					<view class="d-flex a-center info mt-2 font-26 color6">
						<view class="phone d-flex a-center mr-5">
							<image src="../../static/images/service-icon.png">
							<view class="ml-1">{{item.phone}}</view>
						</view>
						<view  class="address d-flex a-center">
							<image src="../../static/images/service-icon1.png">
							<view class="ml-1">{{item.province}}·{{item.city}}</view>
						</view>
					</view>
					<view class="d-flex j-end a-center">
						<view class="d-flex a-center mr-5">
							<view class="mr-1">
								<image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon.png"></image>
							</view>
							<view class="font-26 color6">编辑</view>
						</view>
						<view class="d-flex a-center" @click="deleteThis(index)">
							<view class="mr-1">
								<image style="width: 25rpx;height:25rpx;" src="../../static/images/irelease-icon1.png"></image>
							</view>
							<view class="font-26 color6">删除</view>
						</view>
					</view>
				</view>
			</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				showView: false,
				score: '',
				wx_info: {},
				curr: 1,
				companyDate: [{}, {}],
				wantBuyDate: [{}, {}],
				searchItem: {
					thirdapp_id: 2
				},
				labelData: [],
				mainData: [],
				idArray: []
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData'], self);
		},

		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},

		methods: {
			
			deleteThis(index) {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确定删除此条发布吗？',
					showCancel:true,
					success: function(res) {
						if (res.confirm) {
							const postData = {
								searchItem: {
									id: self.mainData[index].id
								},
								data: {
									status: -1
								}
							};
							postData.tokenFuncName = 'getUserToken';
							const callback = (res) => {
								uni.setStorageSync('canClick', true);
								if (res.solely_code == 100000) {
									self.$Utils.showToast('删除成功', 'none', 1000)
									setTimeout(function() {
										self.getMainData(true)
									}, 1000);
								} else {
									self.$Utils.showToast(res.msg, 'none', 1000)
								};
							};
							self.$apis.articleUpdate(postData, callback);
						} else if (res.cancel) {
							uni.setStorageSync('canClick', true);	
							console.log('用户点击取消');
						}
					}
				});
			},

			tel() {
				wx.makePhoneCall({
					phoneNumber: '15623455454',
				})
			},

			changeCurr(curr) {
				const self = this;
				if (curr != self.curr) {
					self.curr = curr
					if (self.curr == 1) {
						self.searchItem.menu_id = ['in', self.idArray]
					} else if (self.curr == 2) {
						self.searchItem.menu_id = 8
					} else if (self.curr == 3) {
						self.searchItem.menu_id = 10
					};
					self.getMainData(true)
				}
			},

			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					parentid: 9
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data)
						for (var i = 0; i < self.labelData.length; i++) {
							self.idArray.push(self.labelData[i].id)
						}
						self.searchItem.menu_id = ['in', self.idArray]
						self.getMainData()
					}
				};
				self.$apis.labelGet(postData, callback);
			},

			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					spu: {
						tableName: 'SkuLabel',
						middleKey: 'spu_item',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: 'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.articleGet(postData, callback);
			},



		},
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/buy.css";
	@import "../../assets/style/index.css";
	@import "../../assets/style/service.css";

	page {
		background: #F5F5F5;
	}

	.orderNav .tt {
		width: 33.3%;
	}

	button {
		background: none;
		line-height: 1.5;
		height: 80rpx;
		padding: 10rpx 50rpx;
		width: 300rpx;
	}

	button::after {
		border: none;
	}

	.button-hover {
		color: #000000;
		background: none;
	}
</style>
