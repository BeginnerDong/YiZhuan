<template>
	<view>
		<view class="">
			
			<view class="editLine font-26">
				<view class="item d-flex j-sb a-center">
					<view class="ll">企业名称：</view>
					<view class="rr d-flex a-center">
						<input type="text" v-model="submitData.title" placeholder="请输入企业的名称" placeholder-class="placeholder" />
					</view>
				</view>
				
				<view class="item d-flex j-sb a-center">
					<view class="ll">服务区域：</view>
					<view class="rr selt-R d-flex j-end a-center color6 font-26">
						<picker mode="region" @change="chooseAddress">
							<view>{{submitData.province!=''?submitData.province+submitData.city+submitData.country:'请选择'}}</view>
						</picker>
						<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image>
					</view>
				</view>
				
				
				<view class="" v-for="(item,index) in mainData" :key="index">
					<view class="py-3 font-weight mx-3">{{item.menu}}</view>
					<view class="editLine font-26">
						<view class="item d-flex j-sb whether a-start" v-for="(c_item,c_index) in item.data" :key="c_index">
							<view class="ll">{{c_item.title}}：</view>
							<view class="rr selt-R  canDoList d-flex j-end flex-wrap a-start">
								<view class="lis selt-R d-flex j-end ml-4 a-start"  v-if="cc_item.style==1" v-for="(cc_item,cc_index) in c_item.child" :key="cc_index">
									<!-- <view>{{cc_item.style}}</view> -->
									<image class="setIcon mr-1" :data-id="c_item.id" :data-c_id="cc_item.id" 
									@click="choose($event.currentTarget.dataset.id,$event.currentTarget.dataset.c_id)"
									:src="Utils.inArray(cc_item.id,submitData.spu_item)>-1?'../../static/images/add-icon1.png':'../../static/images/add-icon.png'" mode=""></image>
									<view>{{cc_item.title}}</view>
								</view>
								<view class="lis selt-R d-flex j-end"  v-if="cc_item.style==2" v-for="(cc_item,cc_index) in c_item.child" :key="cc_index">
									<input type="text" v-model="text" @blur="inputIn($event.currentTarget.dataset.id)" :data-id="cc_item.id"   placeholder-class="placeholder" />
								</view>
								<view class="rr selt-R d-flex j-end" style="width: 100%;"  v-if="cc_item.style==4" v-for="(cc_item,cc_index) in c_item.child" :key="cc_index">
									<textarea v-model="text" @blur="inputIn($event.currentTarget.dataset.id)" :data-id="cc_item.id"  placeholder="请填写" placeholder-class="placeholder" />
								</view>
							</view>
						</view>
					</view>	
					<view class="f5Bj-H10"></view>
				</view>
			</view>
		</view>
		<view class="">
			<view class="py-3 font-weight mx-3">公司图集</view>
			<view class="mx-3 d-flex a-center pb-2">
				<view class="upImg d-flex j-center a-center overflow-h mr-2 f5bj">
					<view class="" style="width: 100%;height: 100%;" v-for="(item,index) in submitData.mainImg" :key="index">
						<image :src="item.url" mode=""></image>
					</view>
					<view class="uni-hello-addfile" style="width: 50%;height: 50%;" @click="upLoadImg('mainImg')">
						<image src="../../static/images/release-icon.png" mode=""></image>
					</view>
				</view>
				<view class="font-24 color9" v-if="submitData.mainImg.length==0">可上传多张</view>
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		
		
		
		<view class="">
			<view class="py-3 font-weight mx-3">执照资质</view>
			<view class="mx-3 d-flex a-center pb-2">
				<view class="upImg d-flex a-center" style="justify-content: center;"> 
					<view class="" style="width: 100%;height: 100%;" v-for="(item,index) in submitData.bannerImg" :key="index">
						<image :src="item.url" mode=""></image>
					</view>
					<view class="uni-hello-addfile" style="width: 50%;height: 50%;" @click="upLoadImg('bannerImg')">
						<image src="../../static/images/release-icon.png" mode=""></image>
					</view>
				</view>
				<view class="font-24 color9" v-if="submitData.bannerImg.length==0">可上传多张</view>
			</view>
		</view>
		<view class="f5Bj-H10"></view>

		<view class="">
			<view class="py-3 font-weight mx-3">联系方式</view>
			<view class="editLine font-26">
				<view class="item d-flex j-sb a-center">
					<view>联系电话：</view>
					<view class="rr">
						<input type="text" v-model="submitData.phone" placeholder="请填写" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item d-flex j-sb a-center">
					<view>联系人：</view>
					<view class="rr">
						<input type="text" v-model="submitData.name" placeholder="请填写" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		
		<view class="submitbtn pb-5" style="margin-top: 80rpx;">
			<button class="btn" type="button" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确定</button>
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:[],
				submitData:{
					title:'',
					name:'',
					phone:'',
					mainImg:[],
					bannerImg:[],
					province:'',
					city:'',
					country:'',
					spu_item:[],
					spu_content:{}
				},
				middleArray:{},
				middleInput:{},
				text:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.submitData.menu_id = self.id;
			if(options.art_id){
				self.getArtData(options.art_id)
			}
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
			getArtData(id) {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id: id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.artData = res.info.data[0]
						self.submitData.title = self.artData.title;
						self.submitData.name = self.artData.name;
						self.submitData.phone = self.artData.phone;
						self.submitData.mainImg = self.artData.mainImg;
						self.submitData.bannerImg = self.artData.bannerImg;
						self.submitData.city = self.artData.city;
						self.submitData.province = self.artData.province;
						self.submitData.country = self.artData.country;
						self.submitData.spu_item = self.artData.spu_item;
						self.submitData.spu_content = self.artData.spu_content;
					}
				};
				self.$apis.articleGet(postData, callback);
			},
			
			chooseAddress(e){
				const self = this;
				self.submitData.province = e.detail.value[0];
				self.submitData.city = e.detail.value[1];
				self.submitData.country = e.detail.value[2];
			},
			
			
			
			choose(id,c_id){
				const self = this;
				self.submitData.spu_item = [];
				self.middleArray[id] = c_id;
				console.log(self.middleArray)
				for(let i in self.middleArray){
					self.submitData.spu_item.push(self.middleArray[i])
				}
				console.log(self.submitData.spu_item)
			},
			
			inputIn(e){
				const self = this;
				self.submitData.spu_content[e] = self.text;
				console.log(self.submitData.spu_content)
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					const callback = (user, res) => {
						console.log(res)
						self.articleAdd();
					};
					self.$Utils.getAuthSetting(callback);
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			articleAdd() {
				const self = this;
				
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = self.$Utils.cloneForm(self.submitData);
				
				const callback = (res) => {
					uni.setStorageSync('canClick',true);
					if(res.solely_code==100000){
						self.userInfoUpdate(res.info.id)
					}
				};
				self.$apis.articleAdd(postData, callback);
			},
			
			userInfoUpdate(id) {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					company:id
				};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (data) => {
					if (data.solely_code == 100000) {
						self.$Utils.showToast('添加成功', 'none', 1000)
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0];
						self.getSkuLabelData()
					}
				};
				self.$apis.labelGet	(postData, callback);
			},
			
			getSkuLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:['in',self.labelData.spu_array]
				};
				postData.getAfter = {
					child:{
						tableName:'SkuLabel',
						middleKey:'id',
						key:'parentid',
						searchItem:{
							status:1
						},
						condition:'in',
						order:{
							listorder:'desc'
						}
					}
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						for (var i = 0; i < res.info.data.length; i++) {
							if (self.mainData.length > 0) {
								var hasone = false;
								for (var j = 0; j < self.mainData.length; j++) {
									if (res.info.data[i].group == self.mainData[j].menu) {
										self.mainData[j].data.push(res.info.data[i]);
										hasone = true;
									};
								};
								if (!hasone) {
									self.mainData.push({
										menu: res.info.data[i].group,
										data: [res.info.data[i]],
						
									});
								};
							} else {
								self.mainData.push({
									menu: res.info.data[i].group,
									data: [res.info.data[i]],
								})
							};
						};
						console.log('mainData',self.mainData)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.skuLabelGet(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;	
				if (self.submitData[type].length > 8) {
					api.showToast('仅限9张', 'fail');
					return;
				};
				wx.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log('type',type)
						console.log(self.submitData)
						wx.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 9,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths;
						console.log(callback)
						for (var i = 0; i < tempFilePaths.length; i++) {
							var file = res.tempFiles[i];
							var obj = res.tempFiles[i].path.lastIndexOf(".");
							var ext = res.tempFiles[i].path.substr(obj+1);
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'img'
							}, callback)
						}
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/canDo.css";	
	.w200{width: 200rpx;position: relative;}
	.editLine .item .w200 input{padding-right:50rpx;}
	.w200 input:after{content: '万';font-size: 24rpx;color: #999;position: absolute; right: 20rpx;top: 2rpx;}
	
	
	.editLine .item .ll{text-align: left;}
	.editLine .item .ll .red{margin-right: 10rpx;}
	.canDoList.hotLable .lis{padding-right: 40rpx;width: 45%;}
	.canDoList.hotLable .lis:nth-of-type(2n){padding-right: 0;}
	
	.editLine .item.whether{line-height: 40rpx;}
	.editLine .item.whether .ll{width: 25%;}
	.editLine .item.whether .rr{width: 70%;}
	.editLine .item.whether .canDoList .lis{padding: 0;}
	
	.setIcon{width: 30rpx;height: 30rpx;display: block;margin-right: 10rpx;}
	.uni-hello-addfile{width: 100%;height: 100%;}
</style>
