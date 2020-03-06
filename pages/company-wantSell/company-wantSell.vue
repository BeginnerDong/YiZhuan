<template>
	<view>
		<view class=" ">
			<view class="pdtb15 ftw mglr4">基本信息</view>
			
			<view class="editLine fs13">
				<view class="item flexRowBetween">
					<view class="ll">公司类型：</view>
					<view class="rr flexEnd">
						<view class="flex mgr20" @click="seltType('1')">
							<image class="setIcon" :src="currType==1?'../../static/images/add-icon1.png':'../../static/images/add-icon.png'" mode=""></image>
							<view>空壳公司</view>
						</view>
						<view class="flex" @click="seltType('2')">
							<image class="setIcon" :src="currType==2?'../../static/images/add-icon1.png':'../../static/images/add-icon.png'" mode=""></image>
							<view>实体公司</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">公司名称：</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入完整公司名称" placeholder-class="placeholder" />
					</view>
				</view>
				
				<view class="item flexRowBetween">
					<view class="ll flex" style="line-height: 36rpx;">
						
						<view>社会信用代码：</view>
					</view>
					<view class="rr">
						<input type="text" value="" placeholder="请填写正确的社会信用代码" placeholder-class="placeholder" />
					</view>
				</view>
				
				<view class="item flexRowBetween">
					<view class="ll">注册资本：</view>
					<view class="rr">
						<input type="text" value="" placeholder="请填写" placeholder-class="placeholder" />
					</view>
				</view>
				
				<view class="item flexRowBetween">
					<view class="ll">实缴资本：</view>
					<!-- <view class="rr flexEnd">
						<view class="w200"><input type="text" value=" " placeholder=" " placeholder-class="placeholder" /></view>
						<view class="selt-R mgl15">
							<picker class="" @change="actualBayChange" :value="bayIndex" :range="bayArray">
								<view class="adrs flexRowBetween">
									<view class="text">{{bayArray[bayIndex]}}</view>
									
								</view>
							</picker>
						</view>
					</view> -->
					<view class="rr">
						<input type="text" value="" placeholder="请填写" placeholder-class="placeholder" />
					</view>
				</view>
				
				<view class="item flexRowBetween">
					<view class="ll">成立日期：</view>
					<view class="rr">
						<input type="text" value="" placeholder="以执照上的成立日期为准" placeholder-class="placeholder" />
					</view>
				</view>
				
				<view class="item flexRowBetween">
					<view class="ll">经营范围：</view>
					<view class="rr selt-R flex">
						<textarea value="" placeholder="完整的经营范围更便于快速卖出,请于执照上的经营范围保持一致" placeholder-class="placeholder" />
					</view>
				</view>
				
				<view class="item flexRowBetween">
					<view class="ll">注册地址：</view>
					<view class="rr selt-R flexEnd">
						<picker mode="region" >
							<view>请选择</view>
						</picker>
					</view>
				</view>
				
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="">
			<view class="pdtb15 ftw mglr4">资产信息</view>
			<view class="editLine fs13">
				<view class="item flexRowBetween">
					<view class="ll">热门标签：</view>
					<view class="rr flex canDoList hotLable">
						<view class="lis flexEnd" v-for="(item,index) in canDoDate" :key="index" @click="canDoChange(index)">
							<image class="setIcon" :src="item.check==true?'../../static/images/add-icon1.png':'../../static/images/add-icon.png'" mode=""></image>
							<view>{{item.title}}</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween whether">
					<view class="ll">是否有资质：</view>
					<view class="rr flexEnd canDoList">
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>有</view>
						</view>
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>否</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween whether">
					<view class="ll w45">是否有商标：</view>
					<view class="rr w55 flexEnd canDoList">
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>有</view>
						</view>
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>否</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween whether">
					<view class="ll w45">是否有网络资产：</view>
					<view class="rr w55 flexEnd canDoList">
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>有</view>
						</view>
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>否</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween whether">
					<view class="ll w45">是否有车牌指标：</view>
					<view class="rr w55 flexEnd canDoList">
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>有</view>
						</view>
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>否</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween whether">
					<view class="ll w45">是否有专利/著作权：</view>
					<view class="rr w55 flexEnd canDoList">
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>有</view>
						</view>
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>否</view>
						</view>
					</view>
				</view>
			</view>	
		</view>
		<view class="f5H5"></view>
		
		<view class="">
			<view class="pdtb15 ftw mglr4">税务信息</view>
			<view class="editLine fs13">
				<view class="item flexRowBetween whether">
					<view class="ll w45">是否报到/开业：</view>
					<view class="rr w55 flexEnd canDoList">
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>有</view>
						</view>
						<view class="lis flexEnd">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>否</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="">
			<view class="pdtb15 ftw mglr4">银行信息</view>
			<view class="editLine fs13">
				<view class="item flexRowBetween">
					<view class="ll">银行账户：</view>
					<view class="rr flexEnd">
						<view class="flex mgr20">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>已开基本户</view>
						</view>
						<view class="flex">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>未开基本户</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="">
			<view class="pdtb15 ftw mglr4">装让信息</view>
			<view class="editLine fs13">
				<view class="item flexRowBetween">
					<view class="ll">转让价格：</view>
					<view class="rr flexEnd">
						<view class="flex mgr20">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>免费转</view>
						</view>
						<view class="flex mgr20">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>面议</view>
						</view>
						<view class="flex">
							<image class="setIcon" src="../../static/images/add-icon.png" mode=""></image>
							<view>一口价</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">其他信息：</view>
					<view class="rr flexEnd">
						<textarea value="" placeholder="未尽信息或公司资产,可再次详细描述" placeholder-class="placeholder" />
					</view>
				</view>
				
			</view>
		</view>
		<view class="f5H5"></view>
		
		
		<view class="">
			<view class="pdtb15 ftw mglr4">联系方式</view>
			<view class="editLine fs13">
				<view class="item flexRowBetween">
					<view class="ll">联系电话：</view>
					<view class="rr flexEnd">
						<input type="text" value=" " placeholder="" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">联系人：</view>
					<view class="rr flexEnd">
						<input type="text" value=" " placeholder="" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="">
			<view class="pdtb15 ftw mglr4">悬赏设置</view>
			<view class="editLine fs13">
				<view class="item flexRowBetween">
					<view class="ll" style="width: 35%;">是否设置悬赏：</view>
					<view class="rr flexEnd canDoList">
						<view class="lis flexEnd" @click="seltCurr('1')">
							<image class="setIcon" :src="curr==1?'../../static/images/add-icon1.png':'../../static/images/add-icon.png'" mode=""></image>
							<view>是</view>
						</view>
						<view class="lis flexEnd" @click="seltCurr('2')">
							<image class="setIcon" :src="curr==2?'../../static/images/add-icon1.png':'../../static/images/add-icon.png'" mode=""></image>
							<view>否</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">悬赏金额：</view>
					<view class="rr flexEnd">
						<input type="number" value="" placeholder="请输入悬赏金额(每次分享的金额)" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">悬赏次数：</view>
					<view class="rr flexEnd">
						<input type="text" value="" placeholder="请输入悬赏次数" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="submitbtn pdb25" style="margin-top: 80rpx;">
			<button class="btn" type="button">确定</button>
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
				currType:0,
				curr:0,
				canDoDate:[
					{check:false,title:'买过社保'},
					{check:false,title:'银行流水大'},
					{check:false,title:'政府/国企背景'},
					{check:false,title:'外资背景'},
					{check:false,title:'国家局名称'}
				],
				index: 0,
				array:['人民币','美元','韩元','英镑'],
				bayIndex:0,
				bayArray:['人民币','美元','韩元','英镑'],
				checked:false
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			seltType(currType){
				const self = this;
				if(currType!=self.currType){
					self.currType = currType
				}
			},
			canDoChange(i){
				const self = this;
				self.canDoDate[i].check = !self.canDoDate[i].check
			},
			seltCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			// 注册资本
			registerZBChange(e) {
				const self = this;
				self.index = e.target.value
			},
			// 实际缴纳
			actualBayChange(e) {
				const self = this;
				self.bayIndex = e.target.value
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
	.editLine .item.whether .ll{width: 45%;}
	.editLine .item.whether .rr{width: 55%;}
	.editLine .item.whether .canDoList .lis{padding: 0;}
	
	.setIcon{width: 20rpx;height: 20rpx;display: block;margin-right: 10rpx;}
	
	/* checkbox{display: none;} */
</style>
