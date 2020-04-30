<template>
	<view class="posterContainer" catchtouchmove="true">
		<!-- 海报 -->
		<canvas canvas-id="zwyPoster" :style="{ width: cansWidth+'px', height: cansHeight+'px' }" class="isCan"></canvas>
		<view class="bg" @click="closeCans"></view>
		<view class="fixedBox">
			<view class="boxLeft"><button class="flexBtn btnLeft" hover-class="btnHover" @click="saveCans">保存</button></view>
			<view class="boxRight"><button class="flexBtn btnRight" hover-class="btnHover" @click="closeCans">关闭</button></view>
		</view>
	</view>
</template>

<script>
export default {
	props: {},
	data() {
		return {
			// 海报
			cansWidth: 270, //海报宽度
			cansHeight: 430 //海报高度
			// 海报
		};
	},
	created() {
		this.ctx = wx.createCanvasContext('zwyPoster', this);
		var that = this;
		wx.getSystemInfo({
			success(res) {
				let ratio = (res.windowWidth - 80) / 300;
				that.cansHeight *= ratio;
				that.cansWidth *= ratio;
				console.log(res);
			}
		});
	},
	computed: {
		today() {
			let year = new Date().getFullYear();
			let month = new Date().getMonth() + 1;
			let date = new Date().getDate();
			let time = year + '年' + month + '月' + date + '日';
			return time;
		}
	},
	methods: {
		// 绘制海报
		drawPoster(isList) {
			/*
					绘制图片
					网络图片使用 this.drawWebBgTest(),可异步.then() 
					本地图片使用 this.drawBg()
					参数相同
					调用方式:this.drawBg({
						url:'',路径，必须
						sLeft:0~1 | 'center' 百分比离左边距离 1则为100vw,必须
						sTop:0~1 百分比离顶部距离,必须
						sWidth:0~1 百分比宽度，必须
						sHeight:0~1 百分比高度，可不填，不填则默认使用sWidth的值
						r:Number 可不填，图片使用圆角矩形模式
					})
			*/
			this.drawWebBgTest({
				url: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1882299595,1923935642&fm=26&gp=0.jpg',
				sLeft: 0,
				sTop: 0,
				sWidth: 1,
				sHeight: 1
			}).then(() => {
				// this.drawWebBgTest({ url: 'https://img0.zuipin.cn/mp_zuipin/poster/hch-pro.jpg', sLeft: 0.1, sTop: 0.8, sWidth: 0.2, sHeight: 0.15,r:5});
				this.drawBg({url:'/static/scan.jpg',sLeft:.1,sTop:0.80,sWidth:.25,sHeight:0.15,r:5})
				this.circleImg('https://wx.qlogo.cn/mmopen/vi_32/tQTicYLeq4icmGWyd95tXUAZt7ibMpWqAvt2Df8MykLtfJM2D1oN5rueJno94qkSZeFLDlkha2MxFWzWn0y8AdgIA/132', 0.35, 0.23, 12);
				/*
						绘制头像
						绘制头像需要添加域名白名单	downloadFile合法域名	https://wx.qlogo.cn
						调用方式:this.circleImg(img, x, y, r)
						img:图片路径
						x:0~1 百分比离左边距离,
						y:0~1 百分比离顶部距离
						r:0~1 || Number  半径
					*/
				// this.circleImg('https://wx.qlogo.cn/mmopen/vi_32/tQTicYLeq4icmGWyd95tXUAZt7ibMpWqAvt2Df8MykLtfJM2D1oN5rueJno94qkSZeFLDlkha2MxFWzWn0y8AdgIA/132',.35,.23, 12)
				/*
						绘制单行文本
						调用方式:this.drawText({
							text:'文本', String || Array
							sLeft:0~1 || 'center' 百分比离左边距离,
							sTop:0~1 百分比离顶部距离,
							fontSize:Number 文字大小,
							color:'' 颜色,
							bold:Boolean 粗体,
							lineHeight:Number //如果是数组则设置行高
						})
					*/
				// this.drawText({text:'我是标题',sLeft:'center',sTop:0.1,fontSize:12,color:'#5A390F'})
				this.drawText({ text: '我是标题', sLeft: 'center', sTop: 0.16, fontSize: 12, color: '#5A390F' });
				this.drawText({ text: '你的名字', sLeft: 0.48, sTop: 0.265, fontSize: 12, color: '#5A390F' });
				this.drawText({ text: 'zwyboom', sLeft: 0.67, sTop: 0.53, fontSize: 10, color: '#5A390F' });
				this.drawText({ text: this.today, sLeft: 0.64, sTop: 0.58, fontSize: 10, color: '#5A390F' });
				this.drawText({ text: '有些人走着走着就没了', sLeft: 0.1, sTop: 0.65, fontSize: 10, color: '#5A390F', bold: true });
				this.drawText({ text: ['XXXXXXXXXXXXXXXX', 'XXXXXXXXX', 'XXXXXXXX'], sLeft: 0.37, sTop: 0.85, fontSize: 10, color: '#5A390F', lineHeight: 12 });
				// 绘制多行文本
				this.drawPara({
					para: '恭喜您获得了5元兰博基尼优惠劵！兑换码XXXX高1亮1文1字XXXXXX高2亮2文2字XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX', //文本
					fontSize: 10, //字体大小
					sLeft: 0.06, //百分比离左边距离
					sTop: 0.32, //百分比李作边距离
					titleHeight: 16, //一行高度
					sMaxWidth: 0.84, //一行最大宽度
					redWord: ['高1亮1文1字', '高2亮2文2字', '兰博基尼优惠劵'], //关键字
					color: '#5A390F', //正常颜色
					redColor: '#DD524D', //高亮颜色
					bold: false //是否加粗
				});
			});
		},
		closeCans() {
			this.$parent.posterShow = false;
		},
		saveCans() {
			console.log('保存');
			wx.showLoading({
				title: '保存ing...',
				mask: true
			});
			wx.canvasToTempFilePath(
				{
					x: 0,
					y: 0,
					width: this.cansWidth * 1.5,
					height: this.cansHeight * 1.5,
					destWidth: this.cansWidth * 3,
					destHeight: this.cansHeight * 3,
					canvasId: 'zwyPoster',
					success: function(res) {
						wx.hideLoading();
						wx.saveImageToPhotosAlbum({
							filePath: res.tempFilePath,
							success: function(res) {
								wx.showToast({
									title: '保存相册成功'
								});
								console.log('save success');
							},
							fail(res) {
								console.log(res);
								if (res.errMsg == 'saveImageToPhotosAlbum:fail auth deny') {
									wx.showModal({
										title: '您需要授权相册权限',
										success(res) {
											if (res.confirm) {
												wx.openSetting({
													success(res) {},
													fail(res) {
														console.log(res);
													}
												});
											}
										}
									});
								}
							}
						});
					},
					fail(res) {
						wx.hideLoading();
					}
				},
				this
			);
		},
		circleImg(img, x, y, r) {
			var that = this;
			return new Promise((resolve, reject) => {
				wx.getImageInfo({
					src: img,
					success(res) {
						x = Math.ceil(that.cansWidth * x);
						y = Math.ceil(that.cansHeight * y);
						r = r > 1 ? r : Math.ceil(that.cansWidth * r);
						that.ctx.save();
						var d = 2 * r;
						var cx = x + r;
						var cy = y + r;
						that.ctx.arc(cx, cy, r, 0, 2 * Math.PI);
						that.ctx.clip();
						that.ctx.drawImage(res.path, x, y, d, d);
						that.ctx.restore();
						that.ctx.draw(true);
						resolve();
					}
				});
			});
		},
		drawPara(item) {
			var redIndexObj = {};
			if (item.redWord.length > 0) {
				for (var i = 0; i < item.redWord.length; i++) {
					let startIndex = 0,
						index;
					while ((index = item.para.indexOf(item.redWord[i], startIndex)) > -1) {
						redIndexObj[index] = true;
						for (var j = 0; j < item.redWord[i].length; j++) {
							redIndexObj[index + j] = true;
						}
						startIndex = index + 1;
					}
				}
			}
			this.ctx.font = `normal normal ${item.fontSize}px sans-serif`;
			this.ctx.setFillStyle(item.color);
			var isLeft;
			if (item.sLeft == 'center') {
				isLeft = this.cansWidth * (0.5 - item.sMaxWidth / 2) - item.fontSize;
			} else {
				isLeft = item.sLeft * this.cansWidth;
			}
			var maxWidth = Math.floor(this.cansWidth * item.sMaxWidth);
			var tempList = item.para.split('');
			var nowText = '',
				isCol = 0,
				textWidth = 0;
			for (var i = 0; i < tempList.length; i++) {
				if (i > 0) {
					nowText += tempList[i - 1];
				} else {
					nowText += tempList[0];
				}
				textWidth = this.ctx.measureText(nowText).width;
				if (textWidth > maxWidth) {
					isCol++;
					nowText = '占';
					textWidth = this.ctx.measureText(nowText).width;
				}
				if (redIndexObj[i]) {
					this.ctx.save();
					if (item.bold) this.ctx.font = `normal bold ${item.fontSize}px sans-serif`;
					this.ctx.setFillStyle(item.redColor);
				}
				this.ctx.fillText(tempList[i], isLeft + textWidth, item.sTop * this.cansHeight + item.titleHeight * isCol);
				this.ctx.restore();
			}
			this.ctx.draw(true);
		},
		drawBg(item) {
			let x, y, w, h, r;
			y = this.cansHeight * item.sTop;
			w = this.cansWidth * item.sWidth;
			h = item.sHeight ? this.cansHeight * item.sHeight : w;
			if (item.sLeft == 'center') {
				x = this.cansWidth * (0.5 - item.sWidth / 2);
			} else {
				x = this.cansWidth * item.sLeft;
			}
			if (item.r) {
				r = item.r;
				this.ctx.save();
				if (w < 2 * r) r = w / 2;
				if (h < 2 * r) r = h / 2;
				this.ctx.beginPath();
				this.ctx.moveTo(x + r, y);
				this.ctx.arcTo(x + w, y, x + w, y + h, r);
				this.ctx.arcTo(x + w, y + h, x, y + h, r);
				this.ctx.arcTo(x, y + h, x, y, r);
				this.ctx.arcTo(x, y, x + w, y, r);
				this.ctx.closePath();
				this.ctx.clip();
				this.ctx.drawImage(item.url, x, y, w, h);
				this.ctx.restore(); // 返回上一状态
				this.ctx.draw(true);
			} else {
				this.ctx.drawImage(item.url, x, y, w, h);
				this.ctx.draw(true);
			}
		},
		drawWebBgTest(item) {
			var that = this;
			return new Promise((resolve, reject) => {
				wx.getImageInfo({
					src: item.url,
					success(res) {
						let x, y, w, h, r;
						y = that.cansHeight * item.sTop;
						w = that.cansWidth * item.sWidth;
						h = item.sHeight ? that.cansHeight * item.sHeight : w;
						if (item.sLeft == 'center') {
							x = that.cansWidth * (0.5 - item.sWidth / 2);
						} else {
							x = that.cansWidth * item.sLeft;
						}
						if (item.r) {
							r = item.r;
							// 开始绘制
							that.ctx.save();
							that.ctx.beginPath();
							that.ctx.arc(x + r, y + r, r, Math.PI, Math.PI * 1.5);
							that.ctx.moveTo(x + r, y);
							that.ctx.lineTo(x + w - r, y);
							that.ctx.lineTo(x + w, y + r);
							that.ctx.arc(x + w - r, y + r, r, Math.PI * 1.5, Math.PI * 2);
							that.ctx.lineTo(x + w, y + h - r);
							that.ctx.lineTo(x + w - r, y + h);
							that.ctx.arc(x + w - r, y + h - r, r, 0, Math.PI * 0.5);
							that.ctx.lineTo(x + r, y + h);
							that.ctx.lineTo(x, y + h - r);
							that.ctx.arc(x + r, y + h - r, r, Math.PI * 0.5, Math.PI);
							that.ctx.lineTo(x, y + r);
							that.ctx.lineTo(x + r, y);
							that.ctx.closePath();
							that.ctx.clip();
							that.ctx.drawImage(res.path, x, y, w, h);
							that.ctx.restore(); // 返回上一状态
							that.ctx.draw(true);
						} else {
							that.ctx.drawImage(res.path, x, y, w, h);
							that.ctx.draw(true);
						}
						resolve();
					}
				});
			});
		},
		drawTitle(item) {
			let isLeft;
			this.ctx.save();
			if (item.bold) this.ctx.font = `normal bold ${item.fontSize}px sans-serif`;
			this.ctx.setFillStyle(item.color);
			let tempSize = (6 / item.text.length) * item.fontSize * 1.3;
			if (tempSize < item.fontSize) {
				if (tempSize < 10) {
					item.fontSize = 10;
				} else {
					item.fontSize = tempSize;
				}
			}
			this.ctx.setFontSize(item.fontSize);
			if (item.sLeft == 'center') {
				let textWidth = this.ctx.measureText(item.text).width;
				isLeft = this.cansWidth * 0.5 - textWidth / 2;
			} else {
				isLeft = item.sLeft * this.cansWidth;
			}
			if (item.text instanceof Array) {
				if (!item.lineHeight) item.lineHeight = item.fontSize + 2;
				console.log('我是数组', item.text);
				for (var i = 0; i < item.text.length; i++) {
					this.ctx.fillText(item.text[i], isLeft, item.sTop * this.cansHeight + item.lineHeight * i);
				}
			} else {
				console.log('我是字符串', item.text);
				this.ctx.fillText(item.text, isLeft, item.sTop * this.cansHeight);
			}
			this.ctx.draw(true);
			this.ctx.restore();
		},
		drawText(item) {
			var isLeft;
			this.ctx.save();
			if (item.bold) this.ctx.font = `normal bold ${item.fontSize}px sans-serif`;
			this.ctx.setFillStyle(item.color);
			this.ctx.setFontSize(item.fontSize);
			if (item.sLeft == 'center') {
				let textWidth = this.ctx.measureText(item.text).width;
				isLeft = this.cansWidth * 0.5 - textWidth / 2;
			} else {
				isLeft = item.sLeft * this.cansWidth;
			}
			if (item.text instanceof Array) {
				if (!item.lineHeight) item.lineHeight = item.fontSize + 2;
				console.log('我是数组', item.text);
				for (var i = 0; i < item.text.length; i++) {
					this.ctx.fillText(item.text[i], isLeft, item.sTop * this.cansHeight + item.lineHeight * i);
				}
			} else {
				console.log('我是字符串', item.text);
				this.ctx.fillText(item.text, isLeft, item.sTop * this.cansHeight);
			}
			this.ctx.draw(true);
			this.ctx.restore();
		}
	}
};
</script>

<style lang="scss" scoped>
.bg {
	width: 100vw;
	height: 100vh;
	position: fixed;
	left: 0;
	top: 0;
	z-index: 998;
	background-color: rgba(0, 0, 0, 0.8);
}
.fixedBox {
	width: 100%;
	height: 100rpx;
	position: fixed;
	bottom: 30rpx;
	left: 0;
	display: flex;
	z-index: 1000;
	.boxLeft,
	.boxRight {
		width: 50%;
		height: 100rpx;
		display: flex;
		align-items: center;
		position: relative;
		z-index: 1000;
		justify-content: center;
		.flexBtn {
			position: relative;
			z-index: 1000;
			width: 200rpx;
			height: 60rpx;
			text-align: center;
			line-height: 55rpx;
			font-size: 24rpx;
			border-bottom: 6rpx #f58365 solid;
			border-radius: 32rpx;
			color: white;
			background: linear-gradient(to left, #f58365, #ff188a);
		}
	}
}
.btnHover {
	height: 55rpx !important;
	border-bottom: 0 #f6be3c solid !important;
	transform: translateY(3px) translateZ(0px) !important;
}
.isCan {
	border: 6px solid white;
	border-radius: 10px;
	position: fixed;
	left: 0;
	z-index: 999;
	right: 0;
	bottom: 0;
	top: 0;
	margin: auto;
	background-size: 100%;
}
</style>
