	<view>
		<view class="">
			<view class="py-3 font-30 font-weight mx-3">求购信息</view>
			
			<view class="editLine font-26">
				<view class="item d-flex j-sb a-start">
					<view class="ll">求购需求：</view>
					<view class="rr selt-R d-flex">
						<textarea v-model="submitData.title" placeholder="请详情描述求购的公司要求,限100字以内!" />
					</view>
				</view>
				<!-- <view class="item d-flex j-sb a-center">
					<view class="ll d-flex j-sb a-center">公司类型：</view>
					<view class="rr flex canDoList hotLable">
						<view class="lis flexEnd" v-for="(item,index) in typeData" :key="index" @click="typeChange(index)">
							<image class="setIcon" :src="item.check==true?'../../static/images/add-icon1.png':'../../static/images/add-icon.png'" mode=""></image>
							<view class="color6">{{item.title}}</view>
						</view>
					</view>
				</view> -->
				<view class="item d-flex j-sb a-center">
					<view class="ll">求购区域：</view>
					<view class="rr selt-R d-flex j-end a-center color6 font-26">
						<picker mode="region" @change="chooseAddress">
							<view>{{submitData.province!=''?submitData.province+submitData.city+submitData.country:'请选择'}}</view>
						</picker>
						<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image>
					</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H10"></view>
		
		<view class="">
			<view class="pt-3 font-30 font-weight mx-3">联系方式</view>
			<view class="editLine font-26">
				<view class="item d-flex j-sb a-center">
					<view class="ll">联系电话：</view>
					<view class="rr">
						<input type="text" v-model="submitData.phone" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item d-flex j-sb a-center">
					<view class="ll">联系人：</view>
					<view class="rr">
						<input type="text" v-model="submitData.name" placeholder="请输入" placeholder-class="placeholder" />
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
				showView: false,
				wx_info:{},
				is_show:false,
				curr:0,
				check:false,
				typeData:[
					{check:false,title:'实体公司'},
					{check:false,title:'空壳公司'},
					{check:false,title:'实体公司'},
					{check:false,title:'空壳公司'},
					{check:false,title:'实体公司'},
					{check:false,title:'空壳公司'},
					{check:false,title:'实体公司'},
					{check:false,title:'空壳公司'},
				],
				submitData:{
					title:'',
					name:'',
					phone:'',
					
					province:'',
					city:'',
					country:'',
					
				},
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.submitData.menu_id = self.id;
			if(options.art_id){
				self.getArtData(options.art_id)
			}
			// self.$Utils.loadAll(['getMainData'], self);
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
						self.submitData.province = self.artData.province;
						self.submitData.city = self.artData.city;
						self.submitData.country = self.artData.country;
					}
				};
				self.$apis.articleGet(postData, callback);
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
				};
				self.$apis.articleAdd(postData, callback);
			},
			
			chooseAddress(e){
				const self = this;
				self.submitData.province = e.detail.value[0];
				self.submitData.city = e.detail.value[1];
				self.submitData.country = e.detail.value[2];
			},
			
			typeChange(i){
				const self = this;
				self.typeDate[i].check = !self.typeDate[i].check
			},
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/canDo.css";	
	
</style>
