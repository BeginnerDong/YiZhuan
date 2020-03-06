<template>
	<view>
		<view class="">
			<view class="pdtb15 ftw mglr4">求购信息</view>
			
			<view class="editLine fs13">
				<view class="item flexRowBetween">
					<view class="ll flexRowBetween">求购需求：</view>
					<view class="rr selt-R flex">
						<textarea value="" placeholder="请详情描述求购的公司要求,限100字以内!" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll flexRowBetween">公司类型：</view>
					<view class="rr flex canDoList hotLable">
						<view class="lis flexEnd" v-for="(item,index) in typeData" :key="index" @click="typeChange(index)">
							<image class="setIcon" :src="item.check==true?'../../static/images/add-icon1.png':'../../static/images/add-icon.png'" mode=""></image>
							<view class="color6">{{item.title}}</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll flexRowBetween">求购区域：</view>
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
			<view class="pdtb15 ftw mglr4">联系方式</view>
			<view class="editLine fs13">
				<view class="item flexRowBetween">
					<view class="ll flexRowBetween">联系电话：</view>
					<view class="rr flex">
						<input type="text" value="" placeholder="" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll flexRowBetween">联系人：</view>
					<view class="rr flex">
						<input type="text" value="" placeholder="" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="">
			<view class="pdtb15 ftw mglr4">悬赏设置</view>
			<view class="editLine fs13">
				<view class="item flexRowBetween">
					<view class="ll flexRowBetween" style="width: 35%;">是否设置悬赏：</view>
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
					<view class="ll flexRowBetween">悬赏金额：</view>
					<view class="rr flex">
						<input type="number" value="" placeholder="请输入悬赏金额(每次分享的金额)" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll flexRowBetween">悬赏次数：</view>
					<view class="rr flex">
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
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			seltCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			
			typeChange(i){
				const self = this;
				self.typeDate[i].check = !self.typeDate[i].check
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
	
</style>
