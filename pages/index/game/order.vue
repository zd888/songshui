<template>
	<view class="">
		<view class="u-skeleton">
			<view class="u-skeleton-fillet" style="width: 100%;height: 490rpx;">
				<!-- 	<image :src="order.homepageImg" mode="aspectFill" style="width: 100%;height: 100%;"
					@click="saveImg(order.homepageImg,index)"></image> -->
				<swiper class="swiper" :autoplay="true" interval="5000" duration="500" :circular="true"
					style="width: 100%;height: 490rpx;border-radius: 24rpx;">
					<swiper-item v-for="(item,index) in order.voiceIntroduce" :key='index'
						@click="saveImg(order.voiceIntroduce,index)">
						<image :src="item" mode="aspectFill" style="width: 100%;height: 100%;">
						</image>
					</swiper-item>
				</swiper>
			</view>

			<view class="padding-sm bgs u-skeleton-fillet">
				<view class=" padding bg radius u-skeleton-fillet">
					<view class="flex align-center justify-between" style="width: 100%;">
						<view class="text-xxl" style="color: #FF2D01;" v-if="order.money">
							<text class="text-df ">￥</text>{{order.money}}
						</view>
						<view>已售{{order.salesNum?order.salesNum:'0'}}</view>
					</view>
					<view class="text-lg margin-top-xs margin-bottom-sm u-line-1" style="width: 500rpx;">
						{{order.serviceName}}
					</view>
					<!-- <view class="padding-lr padding-tb-xs text-sm"
						style="background: #F2F2F2;color: #025EFD;border-radius: 55upx;">可用水贝支付，消费水贝29个</view> -->
				</view>

				<view class="margin-top-sm padding bg text-white radius u-skeleton-fillet" v-if="commentList.length">
					<view class="flex align-center justify-between">
						<view class="text-lg">用户评价</view>
						<view class="flex align-center" v-if="commentList.length>0" @click="goComment()">
							<view class="margin-right-xs text-sm">查看全部</view>
							<image src="../../../static/images/my/right.png" style="width: 14upx;height: 22upx;">
							</image>
						</view>
					</view>
					<view v-if="commentList.length<=0" class="margin-top-sm"> 暂无评价</view>
					<view class="margin-tb-sm" v-else>
						<view class="flex justify-between">
							<u-avatar :src="commentList[0].avatar" size="48"></u-avatar>
							<view class="flex-sub margin-left-sm" style="line-height: 46upx;">
								{{commentList[0].userName}}
							</view>
							<view class="flex">
								<u-icon v-for="ite in commentList[0].score" :key='ite' color="#2087fe" name="star-fill">
								</u-icon>
							</view>
						</view>
						<view class="margin-top-sm">{{commentList[0].content}}</view>
					</view>

				</view>
				<view class="margin-top-sm bg radius u-skeleton-fillet">
					<!-- <view class="text-center padding">服务详情</view> -->
					<view v-for="(item,index) in order.detailsImg" :key="index"
						@click="saveImg(order.detailsImg,index)">
						<image :src="item" style="width: 100%;" mode="widthFix"></image>
					</view>

				</view>

			</view>

			<view class="u-skeleton-fillet  foot">
				<view @click="follow" class="padding-left" style="width: 120rpx;">
					<image src="../../../static/images/index/shoucang.png" v-if="!shoucang"
						style="width:43upx;height:41upx;"></image>
					<image src="../../../static/images/index/shoucang1.png" v-if="shoucang"
						style="width: 43upx;height: 41upx;">
					</image>
					<view style="font-size: 22upx;">收藏</view>
				</view>
				<view @click="" class="padding-left-sm" style="width: 120rpx;" @click="openshare">
					<image src="../../../static/images/index/share.png" style="width:37upx;height: 40upx;"></image>
					<view style="font-size: 22upx;">分享</view>
				</view>
				<view class="buttons" v-if="XCXIsSelect!='否'">
					<view class="buttshop" @click="openSku(1)">加入购物车</view>
					<view class="buttorder" @click="openSku(2)">立即下单</view>
					<!-- <view class="buttorder" @click="goNav()">立即下单</view> -->
				</view>
			</view>
		</view>


		<!-- 选择规格弹窗 -->
		<u-popup v-model="skuShow" mode="bottom" :closeable='true' border-radius="20">
			<view>
				<view class="flex padding">
					<image :src="order.homepageImg" mode="aspectFill" style="width: 180upx;height: 180upx;"></image>
					<view class="margin-left-sm">
						<view>{{order.serviceName}}</view>
						<view class="flex align-end">
							<view style="color: #FD3C44;font-size: 38upx;">￥{{prices}}
							</view>
							<!-- <view class="margin-left-sm" style="color: #CCCCCC;text-decoration: line-through;">¥29.2
							</view> -->
						</view>
					</view>
				</view>
				<view class="margin-top-sm padding-lr" v-for="(item,index) in attrList" :key="index">
					<view class="text-bold text-black">{{item.value}}</view>
					<view class="flex margin-tb-sm flex-wrap">
						<view v-for="(ite, ind) in item.detail" :key="ind" @click="skuSel(ite,index,ind)"
							class="margin-bottom-sm">
							<view class="skuBtn" :class="item.goodsId == index && item.attrId == ind?'active': ''">
								{{ite.name}}
							</view>
						</view>
					</view>
				</view>

				<view class="flex margin-top-xl justify-between align-center padding-lr">
					<view class="text-lg" style="color: #1A1A1A;">数量选择</view>
					<view class="flex align-center ">
						<view class="jia" @click="down(number)">-</view>
						<view class="content">{{number}}</view>
						<view class="jian" @click="add(number)">+</view>
					</view>
				</view>
				<view class="flex justify-between padding-lr padding-top">
					<view>
						<view class="text-xl text-bold text-black"><text class="text-sm">￥</text>{{totalprice}}
						</view>
						<view class="detail_describe_text2">{{checkString}}</view>
					</view>
					<!-- <u-number-box v-model="value" :min="1" @change="valChange"></u-number-box> -->
				</view>
				<view class="detail_account_bottom padding" v-if="skubtn==2">
					<view class="addshop" @click="goNav()">立即下单</view>
				</view>
				<view class="detail_account_bottom padding" v-if="skubtn==1">
					<view class="addshop" @click="AddShop()">加入购物车</view>
				</view>
			</view>
		</u-popup>

		<u-popup v-model="show" mode="bottom">
			<view class="margin ">
				<view class="flex align-center">
					<!-- #ifdef H5 -->
					<view class="flex align-center padding-lr radius"
						style="background-color: #FFCB49;color: #FFFFFF; width: 50%;" @click="share()">
						<image src="../../../static/share/zhuanfa.png" style="width: 60upx;height:60upx"></image>
						<button style="background-color: #FFCB49;font-size: 16px;font-weight: bold;color: #FFFFFF; "
							type="default">文案推广</button>
					</view>

					<view class="flex align-center padding-lr radius"
						style="background-color: #557EFD;color: #FFFFFF; width: 50%; margin-left: 40upx;"
						@click="showModal()">
						<image src="../../../static/share/xiazai.png" style="width: 60upx;height:60upx"></image>
						<button type="default"
							style="background-color: #557EFD;font-size: 16px;font-weight: bold;color: #FFFFFF;">生成海报</button>
					</view>
					<!-- #endif -->
					<!-- #ifdef APP-PLUS -->
					<view class="flex align-center padding-lr radius"
						style="background-color: #FFCB49;color: #FFFFFF; width: 50%;" v-if="isShowWxAPPShare=='是'"
						@click="shareWeiXin()">
						<image src="../../../static/share/zhuanfa.png" style="width: 60upx;height:60upx"></image>
						<button type="default"
							style="background-color: #FFCB49;font-size: 16px;font-weight: bold;color: #FFFFFF; ">文案推广</button>
					</view>
					<view class="flex align-center padding-lr radius"
						style="background-color: #FFCB49;color: #FFFFFF; width: 50%;" v-if="isShowWxAPPShare=='否'"
						@click="sharAPPUrl()">
						<image src="../../../static/share/zhuanfa.png" style="width: 60upx;height:60upx"></image>
						<button type="default"
							style="background-color: #FFCB49;font-size: 16px;font-weight: bold;color: #FFFFFF; ">文案推广</button>
					</view>
					<view class="flex align-center padding-lr radius"
						style="background-color: #557EFD;color: #FFFFFF; width: 50%; margin-left: 40upx;"
						@tap="showModal()">
						<image src="../../../static/share/xiazai.png" style="width: 60upx;height:60upx"></image>
						<button type="default"
							style="background-color: #557EFD;font-size: 16px;font-weight: bold;color: #FFFFFF;">生成海报</button>
					</view>
					<!-- #endif -->

					<!-- #ifdef MP-WEIXIN -->
					<view class="flex align-center padding-lr radius"
						style="background-color: #FFCB49;color: #FFFFFF; width: 50%;">
						<image src="../../../static/share/zhuanfa.png" style="width: 60upx;height:60upx"></image>
						<button open-type="share"
							style="background-color: #FFCB49;font-size: 16px;font-weight: bold;color: #FFFFFF;">一键分享</button>
					</view>

					<view class="flex align-center padding-lr radius"
						style="background-color: #557EFD;color: #FFFFFF; width: 50%; margin-left: 40upx;"
						@tap="onSaveImg()">
						<image src="../../../static/share/xiazai.png" style="width: 60upx;height:60upx"></image>
						<button type="default"
							style="background-color: #557EFD;font-size: 16px;font-weight: bold;color: #FFFFFF;">生成海报</button>
					</view>
					<!-- #endif -->


				</view>
			</view>
		</u-popup>
		<!-- 生成海报 -->
		<!-- #ifndef MP-WEIXIN -->
		<tki-qrcode ref="qrcode" :val="url" :size="200" background="#fff" foreground="#000" pdground="#000"
			:onval="true" :loadMake="true" @result="qrR" :show="false"></tki-qrcode>
		<view class="cu-modal" :class="modalName == 'Image' ? 'show' : ''" @tap="hideModal">
			<view class="cu-dialog" v-if="backgroundImage && erweimapath && haibaoShow" @tap="hideModal">
				<view class="bg-img">
					<wm-poster @success="posterSuccess" :imgSrc="backgroundImage" :Referrer="'我的邀请码:'+invitationCode"
						:QrSrc="erweimapath" :Title="order.serviceName" :LineType="false"></wm-poster>
				</view>
			</view>
		</view>
		<!-- #endif -->
		<!-- #ifdef MP-WEIXIN -->
		<view @tap="hideModal" :class="modalName == 'Image' ? 'show' : ''" class="modal"
			style="text-align: center;display: flex;justify-content: center;">
			<view style="width:100%;margin: auto;">
				<image :src="h5SaveImg" mode="widthFix" style="width: 90%;"></image>
			</view>
		</view>
		<canvas canvas-id="poster" class="poster_canvas"></canvas>
		<!-- #endif -->

		<u-skeleton :loading="loading" :animation="true" bgColor="#FFF"></u-skeleton>
	</view>
</template>

<script>
	let settingWritePhotosAlbum = false;
	import tkiQrcode from '@/components/tki-qrcode/tki-qrcode.vue';
	import appShare from '@/utils/share.js';
	import wmPoster from '@/components/wm-poster/wm-posterorders.vue';
	import uQRCode from "../../../js_sdk/Sansnn-uQRCode/uqrcode.js"
	import _app from '../../../js_sdk/QuShe-SharerPoster/QS-SharePoster/app.js';
	import configdata from '../../../common/config.js';
	export default {
		components: {
			tkiQrcode,
			wmPoster,
			// getSharePoster
		},
		onShareAppMessage(res) {
			return {
				path: '/pages/index/game/order?invitation=' + this.invitationCode + '&id=' + this.order
					.id+'&titleName='+this.order.serviceName, //这是为了传参   onload(data){let id=data.id;} 
				title: this.order.serviceName,
				imageUrl: this.order.homepageImg
			}
		},
		data() {
			return {
				game: [],
				loading: true, // 是否显示骨架屏组件
				id: '',
				orderDet: {},
				page: 1,
				limit: 10,
				order: {},
				commentList: [],
				isFollow: false,
				myId: '',
				isVip: false,
				AUDIO: uni.createInnerAudioContext(),
				isPlay: false,

				mobile: '',
				name: '',
				cityaddress: '',
				detailaddress: '',
				latitude: '',
				longitude: '',
				province: '',
				city: '',
				district: '',

				tanIndex: 0,
				number: 1,

				goodsDet: {},
				skuShow: false,
				attrList: [],
				attrLists: [],
				skuList: [],
				skuId: '',
				checkString: '',
				checkStateList: [],
				// price: 0,
				prices: 0,
				totalprice: 0,
				CheckattrValue: false,
				skuMessage: '',
				skubtn: '',

				show: false,
				invitationCode: '',
				url: '',
				poster: {},
				qrShow: false,
				haibaoImg: null,
				haibaoShow: false,
				modalName: '',
				erweimapath: '',
				backgroundImage: '',
				canvasId: 'default_PosterCanvasId',
				shoucang: false,
				titleName: '',
				XCXIsSelect: '否',
			}
		},
		onLoad(option) {
			this.XCXIsSelect = this.$queue.getData('XCXIsSelect');
			this.$queue.showLoading("加载中...");
			this.id = option.id
			if (option.titleName) {
				this.titleName = option.titleName
				uni.setNavigationBarTitle({
					title: option.titleName
				});
			}
			if (option.invitation) {
				this.$queue.setData('inviterCode', option.invitation);
			}
			this.myId = uni.getStorageSync('userId')
			this.isVip = uni.getStorageSync('isVIP')
			this.invitationCode = uni.getStorageSync('invitationCode')
			this.getDet()
			if (this.myId) {
				this.getOrderComment()
				this.getBackImageList()
				this.getshoucang()
			}

			// #ifndef H5
			this.$Request.getT('/app/common/type/25').then(res => {
				if (res.code === 0) {
					if (res.data && res.data.value) {
						this.url = res.data.value;
					}
				}
			});
			//#endif
			// #ifdef H5
			this.$Request.getT('/app/common/type/141').then(res => {
				if (res.code === 0) {
					if (res.data && res.data.value && res.data.value == '是') {
						this.$Request.getT('/app/common/type/25').then(ress => {
							if (ress.code === 0) {
								if (ress.data && ress.data.value) {
									this.url = ress.data.value;
								}
							}
						});
					} else {
						this.url = this.$queue.publicYuMing() + '/pages/index/game/order?invitation=' + this
							.invitationCode + '&id=' + this.id;
					}
				}
			});
			//#endif
		},
		onShow() {

		},
		onReady() {

			this.AUDIO.onEnded(function(res) {
				this.isPlay = false;
			});
		},
		methods: {
			openshare() {
				if (this.myId) {
					this.show = true
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
			},
			//是否收藏
			getshoucang() {
				this.$Request.get("/app/collect/selectCollectOrderTaking", {
					orderTakingId: this.id,
				}).then(res => {
					if (res.code == 0) {
						if (res.data == null) {
							this.shoucang = false
						} else {
							this.shoucang = true
						}
					}
				})
			},
			openSku(index) {
				let token = uni.getStorageSync('token')
				if (token) {
					this.checkStateList = [];
					this.attrList.forEach(res => {
						res.goodsId = -1;
						res.attrId = -1;
						let data = {
							name: ''
						}
						this.checkStateList.push(data);
					})
					this.checkString = ''
					this.CheckattrValue = false

					// this.attrList = this.attrLists;
					if (this.order.goodsAttrList.length != 0) {
						this.skubtn = index
						this.number = 1
						this.totalprice = 0
						this.prices = 0
						this.skuId = ''


						this.skuShow = true
					} else if (this.order.goodsAttrList.length == 0 && index == 2) {
						this.CheckattrValue = true
						this.goNav()
					} else if (this.order.goodsAttrList.length == 0 && index == 1) {
						this.CheckattrValue = true
						this.AddShop()
					}
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
			},
			down(index) {
				if (index == 1) {
					uni.showToast({
						title: '下单数量不能小于1',
						icon: 'none'
					})
					return
				} else {
					this.number--
				}

			},
			add(num) {
				this.number = Number(num) + 1
				this.totalprice = parseFloat(this.prices * this.number).toFixed(0)
				// this.number++
			},

			goComment() {
				uni.navigateTo({
					url: '/my/other/commentList?id=' + this.order.id
				})
			},
			//详情图片放大
			saveImg(imgs, index) {
				// console.log(imgs)
				let that = this;
				let imgArr = imgs
				// imgArr.push(imgs);
				// //预览图片
				uni.previewImage({
					urls: imgArr,
					current: imgArr[index]
				});
			},
			// 地址
			bindAdd() {
				uni.navigateTo({
					url: '../../../my/address/address'
				})
			},
			// 一键导航
			bindGps(latitude, longitude, name) {
				uni.openLocation({
					latitude: latitude - 0, //要去的纬度-地址       
					longitude: longitude - 0, //要去的经度-地址
					name: name, //地址名称
					address: name, //详细地址名称
					success: function() {
						console.log('导航成功');
					},
					fail: function(error) {
						console.log(error)
					}
				});
			},
			// 详情
			getDet() {
				uni.showLoading({
					title: '加载中...',
					icon: 'none'
				});
				this.$Request.get("/app/orderTaking/queryTakingDetails", {
					id: this.id,
					latitude: uni.getStorageSync('latitude'),
					longitude: uni.getStorageSync('longitude')
				}).then(res => {
					uni.hideLoading();
					this.loading = false
					if (res.code == 0 && res.data) {

						this.order = res.data
						this.backgroundImage = this.order.homepageImg
						if (this.order.voiceIntroduce) {
							this.order.voiceIntroduce = this.order.voiceIntroduce.split(",");
						}
						if (this.order.gameName) {
							this.order.gameName = this.order.gameName.split(",");
						}
						if (this.order.detailsImg) {
							this.order.detailsImg = this.order.detailsImg.split(",");
						}

						if (this.order.goodsAttrList.length == 0) {
							this.skuId = this.order.goodsSkuList[0].id
							// if (this.isVip) {
							// 	this.prices = this.order.goodsSkuList[0].skuMemberPrice
							// 	this.totalprice = this.order.goodsSkuList[0].skuMemberPrice
							// } else {
							// 	this.prices = this.order.goodsSkuList[0].skuPrice
							// 	this.totalprice = this.order.goodsSkuList[0].skuPrice
							// }
							this.prices = this.order.goodsSkuList[0].skuPrice
							this.totalprice = this.order.goodsSkuList[0].skuPrice
						}

						if (this.order.goodsSkuList) {
							this.skuList = this.order.goodsSkuList
						}
						if (this.order.goodsAttrList.length != 0) {
							if (this.order.goodsAttrList.length) {
								this.attrList = this.order.goodsAttrList[0] ? this.order.goodsAttrList[0]
									.attrValue : [];
								this.attrList.forEach(res => {
									let data = {
										name: ''
									}
									this.checkStateList.push(data);
									let detail = [];

									if (res.detail.length > 0) {
										res.detail.split(',').forEach(d => {
											let data = {
												name: '',
												state: ''
											}
											data.name = d;
											detail.push(data);
										});
										res.detail = detail;
									}
								})
								this.attrLists = this.attrList;
							} else {
								this.skuId = this.order.goodsAttrList[0].id
							}
						}

						// this.selectFollow()
						// uni.hideLoading();
					} else {
						// this.loading = true
						uni.hideLoading();
						uni.showToast({
							title: '商品已下架',
							icon: 'none',
							duration: 2000
						})
						setTimeout(function() {
							uni.navigateBack()
						}, 2000)

					}
					// this.loading = true;

				});

			},
			// 选择规格
			skuSel(item, index, ind) {
				this.number = 1
				this.tanIndex = index
				this.attrList[this.tanIndex].goodsId = index
				this.attrList[this.tanIndex].attrId = ind
				this.checkStateList[this.tanIndex].name = item.name;
				this.checkString = '';

				this.checkStateList.forEach(d => {
					if (d.name) {
						if (this.checkString) {
							this.checkString = this.checkString + ',' + d.name;
						} else {
							this.checkString = d.name;
						}
					}
				});

				for (var i = 0; i < this.skuList.length; i++) {
					let d = this.skuList[i];
					if (d.detailJson == this.checkString) {

						this.skuId = d.id
						this.skuMessage = d.detailJson
						// if (this.isVip) {
						// 	this.prices = d.skuMemberPrice
						// 	this.totalprice = d.skuMemberPrice
						// } else {
						// 	this.prices = d.skuPrice
						// 	this.totalprice = d.skuPrice
						// }
						this.prices = d.skuPrice
						this.totalprice = d.skuPrice
						this.CheckattrValue = true;
						// } else {
						// 	this.$queue.showToast('库存不足请选择其他规格')
						// }
						break
					} else {
						this.CheckattrValue = false;
					}
				}
			},
			//添加购物车
			AddShop() {
				if (this.CheckattrValue) {
					// console.log('------')
					let token = this.$queue.getData('token');
					if (token) {
						let data = {
							isShopping: 1,
							id: this.order.id,
							type: 1,
							orderNumber: this.number,
							detailJson: this.skuMessage,
							skuId: this.skuId
						}
						this.$Request.get("/app/orders/generateOrder", data).then(res => {
							if (res.code == 0) {
								uni.showToast({
									title: '加入成功',
									icon: 'none'
								})
								this.skuShow = false
								this.skuId = ''
								this.checkString = ''
								// this.tanIndex = 0
								this.attrList[0].goodsId = -1
								this.attrList[0].attrId = -1
								this.CheckattrValue = false;

								// uni.navigateTo({
								// 	url:'/package/pages/shopCart/index'
								// })
							} else {
								uni.showToast({
									title: res.msg,
									icon: 'none'
								})
							}
						});
					} else {
						this.goLoginInfo();
					}
				} else {
					this.$queue.showToast('请选择规格')
				}


			},
			valChange(e) {
				this.value = e.value
			},
			// 评论
			getOrderComment() {
				this.$Request.get("/app/takingComment/selectOrderTakingComment", {
					id: this.id,
					page: this.page,
					limit: this.limit
				}).then(res => {
					if (res.code == 0) {
						this.commentList = [...this.commentList, ...res.data.list]
					}
				});
			},
			goNav() {
				if (this.CheckattrValue) {
					this.skuShow = false
					uni.navigateTo({
						url: "/pages/index/game/orderDet?id=" + this.order.id + '&number=' + this.number +
							'&skuMessage=' + this.skuMessage + '&skuId=' + this.skuId + '&price=' + this.prices
					})
					// this.attrList[this.tanIndex].goodsId = 0
					this.checkString = '';
					this.CheckattrValue = false;

				} else {
					this.$queue.showToast('请选择规格')
				}

			},
			goMsg() {
				let data = {
					userId: this.myId,
					focusedUserId: this.order.userId
				}
				this.$Request.postJson('/app/chat/insertChatConversation ', data).then(res => {
					if (res.code == 0) {
						let id = this.order.userId == res.data.userId ? res.data.focusedUserId : this.order
							.userId
						uni.navigateTo({
							url: '/pages/msg/im?chatConversationId=' + res.data
								.chatConversationId +
								'&byUserId=' + id
						})
					}
				})
			},
			// 关注
			follow() {
				let that = this
				if (that.myId) {
					that.$Request.post("/app/collect/insertCollectOrderTaking", {
						orderTakingId: that.order.id
					}).then(res => {
						that.getshoucang()
						uni.showToast({
							title: res.msg,
							icon: 'none'
						})
						// setTimeout(function() {
						// 	that.getshoucang()
						// }, 500)
					});
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}

			},
			shareWeiXin() {
				this.$Request.getT('/app/common/type/103').then(res => {
					if (res.code === 0) {
						if (res.data && res.data.value) {
							let relationId = this.invitationCode;
							let shareData = {
								shareUrl: this.url,
								shareTitle: res.data.value,
								shareContent: '邀请码：' + relationId + '，' + res.data.value,
								shareImg: this.$queue.publicYuMing() + '/logo.png',
								type: 0
							};
							appShare(shareData, res => {
								console.log('分享成功回调', res);
							});
						}
					}
				});

			},
			posterSuccess(haibaoImg) {
				this.haibaoImg = haibaoImg;
				this.modalName = 'Image';
			},
			showModal() {
				if (!this.haibaoImg) {
					this.show = false
					this.haibaoShow = true;
					this.$queue.showLoading('海报生成中...');
				} else {
					this.modalName = 'Image';
				}
			},
			hideModal() {
				this.modalName = null;
			},
			qrR(path) {
				this.erweimapath = path;
			},
			hideQr() {
				this.qrShow = false;
			},
			async shareFc() {
				let _this = this;
				try {
					const d = await getSharePoster({
						type: 'testShareType',
						backgroundImage: _this.backgroundImage,
						posterCanvasId: _this.canvasId, //canvasId
						delayTimeScale: 20, //延时系数
						drawArray: ({
							bgObj,
							type,
							bgScale
						}) => {
							const dx = bgObj.width * 0.3;
							const fontSize = bgObj.width * 0.045;
							const lineHeight = bgObj.height * 0.04;
							//可直接return数组，也可以return一个promise对象, 但最终resolve一个数组, 这样就可以方便实现后台可控绘制海报
							return new Promise((rs, rj) => {
								rs([{
										type: 'custom',
										setDraw(Context) {
											Context.setFillStyle('black');
											Context.setGlobalAlpha(0.3);
											Context.fillRect(0, bgObj.height - bgObj
												.height * 0.2, bgObj.width, bgObj
												.height * 0.2);
											Context.setGlobalAlpha(1);
										}
									},
									{
										type: 'text',
										fontStyle: 'italic',
										text: '邀请码:' + _this.invitationCode,
										size: fontSize,
										color: 'white',
										alpha: 1,
										textAlign: 'left',
										textBaseline: 'middle',
										infoCallBack(textLength) {
											return {
												dx: bgObj.width - textLength - fontSize,
												dy: bgObj.height - lineHeight * 3
											}
										},
										serialNum: 0,
										id: 'tag1' //自定义标识
									},
									{
										type: 'qrcode',
										text: _this.url,
										size: bgObj.width * 0.2,
										dx: bgObj.width * 0.05,
										dy: bgObj.height - bgObj.width * 0.25
									}
								]);
							})
						},
						setCanvasWH: ({
							bgObj,
							type,
							bgScale
						}) => { // 为动态设置画布宽高的方法，
							_this.poster = bgObj;
						}
					});
					//_app.log('海报生成成功, 时间:' + new Date() + '， 临时路径: ' + d.poster.tempFilePath)
					_this.poster.finalPath = d.poster.tempFilePath;
					_this.qrShow = true;
				} catch (e) {
					_app.hideLoading();
				}
			},
			saveImage() {
				uni.saveImageToPhotosAlbum({
					filePath: this.poster.finalPath,
					success(res) {
						_app.showToast('保存成功');
					}
				})
			},
			share() {
				this.sharurl();
			},
			sharurl() {
				let that = this;
				// this.$queue.showLoading('加载中...');
				that.show = false
				let relationId = this.invitationCode;
				uni.showModal({
					title: '服务分享',
					content: this.order.serviceName + '\n' + this.url,
					showCancel: true,
					cancelText: '关闭',
					confirmText: '一键复制',
					success: res => {
						if (res.confirm) {
							uni.setClipboardData({
								data: this.order.serviceName + '\n' + this.url,
								success: function() {
									console.log('success');
									that.$queue.showToast('复制成功');
								}
							});
						}
					}
				});
			},
			// 微信小程序保存图片
			async onSaveImg() {
				// #ifdef MP-WEIXIN

				let imgUrl = await this.createPoster();
				uni.showLoading({
					title: '海报下载中'
				});
				if (settingWritePhotosAlbum) {
					uni.getSetting({
						success: res => {
							if (res.authSetting['scope.writePhotosAlbum']) {
								uni.saveImageToPhotosAlbum({
									filePath: imgUrl,
									success: () => {
										uni.hideLoading();
										uni.showToast({
											title: '保存成功'
										});
									}
								});
							} else {
								uni.showModal({
									title: '提示',
									content: '请先在设置页面打开“保存相册”使用权限',
									confirmText: '去设置',
									cancelText: '算了',
									success: data => {
										if (data.confirm) {
											uni.hideLoading();
											uni.openSetting();
										}
									}
								});
							}
						}
					});
				} else {
					uni.hideLoading();
					settingWritePhotosAlbum = true;
					uni.authorize({
						scope: 'scope.writePhotosAlbum',
						success: () => {
							uni.saveImageToPhotosAlbum({
								filePath: imgUrl,
								success: () => {
									uni.hideLoading();
									uni.showToast({
										title: '保存成功'
									});
								}
							});
						}
					});
				}
				// #endif
			},
			getBackImageList() {
				this.$Request.getT('/app/banner/selectBannerList?state=-1&classify=5').then(res => {
					if (res.code === 0) {
						// this.backgroundImage = res.data[0].imageUrl;
					}
				});
				this.make();

			},
			make() {
				uQRCode.make({
					canvasId: 'default_PosterCanvasId',
					componentInstance: this,
					text: this.url,
					size: 68,
					margin: 4,
					backgroundColor: '#ffffff',
					foregroundColor: '#000000',
					fileType: 'jpg',
					correctLevel: uQRCode.errorCorrectLevel.H,
					success: res => {
						console.log(res)
					}
				})
			},
			//生成海报
			createPoster() {
				let that = this;
				that.show = false
				return new Promise((resolve, reject) => {
					uni.showLoading({
						title: '海报生成中'
					});
					const ctx = uni.createCanvasContext('poster');
					ctx.fillRect(0, 0, 375, 673);
					ctx.setFillStyle("#FFF");
					ctx.fillRect(0, 0, 375, 673);
					let imgUrl = that.backgroundImage;
					uni.downloadFile({
						url: imgUrl,
						success: (res) => {
							if (res.statusCode === 200) {
								console.log(that.config("APIHOST1") +
									'/app/invite/mpCreateQr?invitationCode=' + that.invitationCode,
									'=========')
								uni.downloadFile({
									url: that.config("APIHOST1") +
										'/app/invite/mpCreateQr?invitationCode=' + that
										.invitationCode,
									success: (res2) => {
										console.log(res2)
										uni.hideLoading();
										if (res.statusCode === 200) {
											ctx.drawImage(res.tempFilePath, 0, 0, 375,
												500);
											// 长按识别二维码访问
											let textTop = 0;
											ctx.setFontSize(19);
											ctx.setFillStyle('#333');
											ctx.fillText("长按识别图中二维码", 17, textTop + 590);
											// 二维码
											ctx.drawImage(res2.tempFilePath, 238, textTop +
												526, 120, 120);
											ctx.draw(true, () => {
												// canvas画布转成图片并返回图片地址
												uni.canvasToTempFilePath({
													canvasId: 'poster',
													width: 375,
													height: 673,
													success: (res) => {
														console.log(
															"海报制作成功！"
														);
														resolve(res
															.tempFilePath
														);
													},
													fail: () => {
														uni
															.hideLoading();
														reject();
													}
												})
											});
										} else {
											uni.hideLoading();
											uni.showToast({
												title: '海报制作失败，图片下载失败',
												icon: 'none'
											});
										}
									},
									fail: err => {
										console.log(err)
										uni.hideLoading();
										uni.showToast({
											title: '海报制作失败，图片下载失败',
											icon: 'none'
										});
									},
									complete: com => {
										console.log(com)
										uni.showToast({
											title: com,
											icon: 'none'
										});
									},
								});
							} else {
								uni.hideLoading();
								uni.showToast({
									title: '海报制作失败，图片下载失败',
									icon: 'none'
								});
							}
						},
						fail: err => {
							// that.yu.toast(err)
							console.log(err)
							uni.hideLoading();
							uni.showToast({
								title: '海报制作失败，图片下载失败',
								icon: 'none',
							});
						}
					});
				});
			},
			config: function(name) {
				var info = null;
				if (name) {
					var name2 = name.split("."); //字符分割
					if (name2.length > 1) {
						info = configdata[name2[0]][name2[1]] || null;
					} else {
						info = configdata[name] || null;
					}
					if (info == null) {
						let web_config = cache.get("web_config");
						if (web_config) {
							if (name2.length > 1) {
								info = web_config[name2[0]][name2[1]] || null;
							} else {
								info = web_config[name] || null;
							}
						}
					}
				}
				return info;
			}
		}

	}
</script>

<style>
	page {
		background-color: #FFFFFF;
	}

	.bgs {
		background-color: #F7F7F7;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.line_s {
		display: inline-flex;
		width: 14rpx;
		height: 14rpx;
		background: #1AD566;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.line_x {
		display: inline-flex;
		width: 14rpx;
		height: 14rpx;
		background: #000000;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.u-size-default {
		background: #557EFD !important;
	}

	.foot {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		z-index: 999;
		background: #FFFFFF;
		padding: 10upx 30upx;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.buttons {
		display: flex;
		align-items: center;

	}

	.buttshop {
		background: #FDC72F;
		border-radius: 40upx 0upx 0upx 40upx;
		padding: 17upx 45upx;
		color: #FFFFFF;

		font-size: 28upx;
	}

	.buttorder {
		background: #0175FE;
		border-radius: 0upx 40upx 40upx 0upx;
		padding: 17upx 45upx;
		color: #FFFFFF;
		font-size: 28upx;
	}

	.labels {
		background: #F6F6F6;
		border-radius: 8upx;
		padding: 10upx 30upx;
		margin-right: 12upx;
		display: inline-block;
	}

	.active {
		padding: 10upx 30upx !important;
		color: #025EFD !important;
		border: 2upx solid #025EFD !important;
	}

	.jia {
		width: 50upx;
		height: 48upx;
		border: 1upx solid #000000;
		text-align: center;
		line-height: 42upx;
	}

	.content {
		width: 55upx;
		height: 48upx;
		/*单侧符合属性设置各边框*/
		border-top: 1upx double #000000;
		border-bottom: 1upx double #000000;
		/* border-left: 10px solid #1a5fad; */
		/* border: 1upx solid #000000; */
		text-align: center;
		line-height: 42upx;
	}

	.jian {
		width: 50upx;
		height: 48upx;
		border: 1upx solid #000000;
		text-align: center;
		line-height: 42upx;
	}

	.addshop {
		background: #0175FE;
		border-radius: 50upx;
		text-align: center;
		color: #FFFFFF;
		padding: 30upx 0upx;
		margin-top: 80upx;
		width: 100%;

	}

	.detail_describe_text2 {
		font-weight: 500;
		margin-top: 2%;
		color: #999999;
	}

	.detail_describe_text2 {
		font-weight: 500;
		margin-top: 2%;
		color: #999999;
	}

	.detail_account_bottom {
		margin-top: 20rpx;
		width: 100%;
		display: flex;
		justify-content: space-between;
		/* margin: 4% 0 3%; */
	}

	.detail_account_bottom_le {
		width: 100%;
		/* margin-right: 5%; */
		border-radius: 44rpx;
		text-align: center;
		background-color: #8139fd;
		color: #fff;
		padding: 20upx 0;
		/* line-height: 2.8; */
		/* border: 2rpx solid #8139fd; */
	}

	.skuBtn {
		padding: 10rpx 30rpx;
		border-radius: 10rpx;
		text-align: center;
		margin-right: 20upx;
		margin-top: 20upx;
		font-size: 28rpx;
		color: #333333;
		border: 2px solid #F2F2F2;
		display: inline-block;

	}

	/* .active {
		background: #EADEFF !important;
		border: 2px solid #8139fd !important;
		color: #8139fd;
		opacity: 0.6;
	} */
	button::after {
		border: none;
	}
	.poster_canvas {
		width: 750upx;
		height: 1334upx;
		position: fixed;
		top: -10000upx;
		left: 0;
	}
</style>