<template>
	<view class="">
		<!-- <view class="padding-lr padding-tb flex align-center justify-between u-skeleton-rect"
			style="background: #FFFFFF;">
			<view class="flex align-center justify-between margin-right-sm" @tap.stop="goSelectCity"
				style="line-height: 68rpx;">
				<image src="../../static/images/index/place.png" style="width: 27rpx;height: 37rpx;" class="">
				</image>
				<view class="localName margin-left-sm text-cut" style="color: #343546;width: 90upx;">{{city}}</view>
			</view>
			<u-search class="flex-sub" placeholder="搜索你需要的服务" shape="square" disabled :show-action="false"
				:animation="true" bg-color="#F7F7F7" color="#1A1A1A" @click="gosearch"></u-search>
		</view> -->
		<view class="zhandian" v-if="isZhandian!='否'">
			<view class="flex align-center">
				<image src="../../static/images/index/place.png" style="width: 27rpx;height: 37rpx;"></image>
				<view class="margin-left-sm">{{laundryName?laundryName:'暂无站点'}}</view>
			</view>
			<view class="text-26" @click="goZhandian()">
				查看更多
				<image src="../../static/images/my/right.png" style="width: 11rpx;height: 20rpx;margin-left: 10rpx;">
				</image>
			</view>
		</view>
		<!-- 服务 -->
		<view class="fw " :style="isZhandian!='否'?'':'margin-top:0;'">
			<view class="fw-box ">
				<swiper class="screen-swiper" style="height: 300rpx;" :circular="true" :autoplay="true" interval="2500"
					duration="800">
					<swiper-item v-for="(item,index) in swiperList" :key="index">
						<image :src="item.imageUrl" mode="widthFix" class="radius"></image>
					</swiper-item>
				</swiper>
			</view>
		</view>

		<view class="margin" v-if="gridData.length>0">
			<u-grid :col="5" :border="false">
				<u-grid-item bg-color="#F4F8FB" v-for="(item,index) in gridData" :key='index' @click="goNav(item.url)">
					<image :src="item.imageUrl" style="width: 92rpx;height: 92rpx;border-radius: 92rpx;">
					</image>
					<view class="grid-text">{{item.name}}</view>
				</u-grid-item>
			</u-grid>
		</view>

		<view class="flex align-center margin-lr " style="background: #FFFFFF;border-radius: 16upx;">
			<view class="flex align-center margin-lr">
				<image src="../../static/images/index/gonggao.png" style="width: 45upx;height: 48upx;"></image>
			</view>
			<view style="width: 1rpx;height: 39upx;background: #CCCCCC;"></view>
			<view>
				<swiper v-if="messageList.length > 0" :autoplay="true" :vertical="true" :interval="4000"
					:circular="true" :indicator-dots="false" class="index_gonggao">
					<block v-for="(item, index) in messageList">
						<swiper-item>
							<view class="flex margin-left">
								<text>{{ item.userName }}刚刚下单</text>
							</view>
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>

		<!-- <view class="flex align-center justify-between margin">
			<view @tap="goNav('/my/gird/water')">
				<image src="../../static/images/index/dingshui.png" style="width: 333upx;height: 180upx;"></image>
			</view>
			<view @tap="goNav('/my/jifen/index')">
				<image src="../../static/images/index/jifen.png" style="width: 333upx;height: 180upx;"></image>
			</view>
		</view> -->
		<!-- <view class=" u-skeleton-rect">
				<me-tabs v-model="tabIndex" nameKey='gameName' :tabs="tabData" @change="tabChange"></me-tabs>
			</view> -->
		<!-- <view class="flex align-center margin-lr justify-between">
			<view class="fiterBox" v-for="(item,index) in filterData" :key="index" @click="fterChange(item.id)"
				:class="defaultIndex==item.id?'active':''">{{item.name}}</view>
		</view> -->

		<view class="margin-lr margin-top" v-for="(list,ind) in List" :key='ind' v-if="list.orderTakingList.length>0">
			<view class="text-center text-lg text-bold">{{list.gameName}}</view>

			<view class="flex flex-wrap  justify-between margin-top">
				<view class="listbox margin-bottom" v-for="(item,index) in list.orderTakingList" :key='index'
					@click="clickItem(item,index)">
					<image :src="item.homepageImg?item.homepageImg:'../../static/logo.png'"></image>
					<view class="padding-lr-sm padding-tb-sm">
						<view class="text-cut" style="width: 300rpx;">{{item.serviceName }}</view>
						<view class="text-sm margin-tb-xs text-cut" style="color: #999999;width: 300rpx;">{{item.myLevel}}</view>
						<view class="flex align-center justify-between">
							<view style="color: #FD3C44;font-size: 38upx;font-weight: bold;">
								￥{{item.money}}</view>
							<!-- <view style="color: #FD3C44;font-size: 38upx;font-weight: bold;">
								￥{{isVip? item.memberMoney :item.money}}</view> -->
							<view style="color: #CCCCCC;font-size: 24upx;">已售{{item.salesNum?item.salesNum:'0'}}</view>
						</view>
					</view>
				</view>
			</view>

		</view>

		<!-- 新人红包 -->
		<u-popup v-model="popushongbao" mode="center">
			<view>
				<image @tap="GetQuan"
					src="https://songshui.xianmaxiong.com/file/uploadPath/2022/11/28/737f1212e0e0c39fbaccc5b9091a2d73.png"
					style="width: 564upx;height:618upx "></image>
			</view>
		</u-popup>
		<!-- <GoodList @click="clickItem" :List="List" :isVip="isVip"></GoodList> -->
		<!-- <empty v-if="List.length == 0"></empty> -->

		<view class="heafbot" @click="goOrdeDet" v-if="isOrder!='否'">
			<image
				src="https://songshui.xianmaxiong.com/file/uploadPath/2023/04/14/cf79e9f05ff31f1289e5fa160cfa2c51.png">
			</image>
		</view>
		<!-- <u-skeleton :loading="loading" :animation="true" elColor='#FFFFFF' bgColor='#FFFFFF'></u-skeleton> -->
	</view>
</template>

<script>
	import MescrollMixin from "@/components/mescroll-uni/components/mescroll-uni/mescroll-mixins.js";
	import mescrollBody from "@/components/mescroll-uni/components/mescroll-body/mescroll-body.vue";
	import meTabs from "@/components/mescroll-uni/me-tabs/me-tabs.vue";
	import empty from '@/components/empty.vue';
	import GoodList from '@/components/good-list/good-list.vue';
	import RenDropdownFilter from '@/components/ren-dropdown-filter/ren-dropdown-filter.vue';

	export default {
		mixins: [MescrollMixin], // 使用mixin
		components: {
			mescrollBody,
			meTabs,
			empty,
			RenDropdownFilter,
			GoodList
		},
		data() {
			return {
				popushongbao: false,
				loading: false, // 是否显示骨架屏组件
				defaultSelected: [],
				tabIndex: 0, // tab下标
				tabData: [{
					createTime: "",
					gameName: '服务推荐',
					gameImg: "",
					id: 0,
					status: 0,
					updateTime: "",
				}],
				swiperList: [],
				gridData: [],
				messageList: [{
					id: 1,
					title: 'sasa'
				}],
				game: [],
				defaultIndex: 1,
				filterData: [{
					id: 1,
					name: '智能推荐'
				}, {
					id: 2,
					name: '价格优先'
				}, {
					id: 3,
					name: '销量优先'
				}, {
					id: 4,
					name: '好评优先'
				}],

				city: '请选择城市',
				latitude: '',
				longitude: '',
				List: [],
				token: '',
				XCXIsSelect: '否',
				isVip: false,
				myId: uni.getStorageSync('userId') ? uni.getStorageSync('userId') : '',
				showModal: true,
				arr: [],
				geRen: 0,
				Qe: 0,
				renzheng: false,
				tuiguang: '',
				backgroundImage: '',
				page: 1,
				limit: 10,
				first: true,
				couponNum: '',
				couponId: '',
				newYouhui: 0,

				first: true,
				couponNum: '',
				couponId: '',
				newYouhui: 0,
				isOrder: '否',

				longitude: '',
				latitude: '',
				laundryName: '',
				laundryId: '',
				isZhandian: '否'
			}
		},
		onShareTimeline(res) {
			let invitationCode = this.$queue.getData('invitationCode') ? this.$queue.getData('invitationCode') : '';
			return {
				path: '/pages/index/index?invitation=' + invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiguang,
				imageUrl: this.backgroundImage
			}
		},
		onShareAppMessage(res) {
			let invitationCode = this.$queue.getData('invitationCode') ? this.$queue.getData('invitationCode') : '';
			return {
				path: '/pages/index/index?invitation=' + invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiguang,
				imageUrl: this.backgroundImage
			}
		},
		onLoad(e) {
			let that = this
			that.getClassfly()
			that.getorderList()
			that.newYouhui = that.$queue.getData('newYouhui');

			this.$Request.getT('/app/banner/selectBannerList?state=-1&classify=5').then(res => {
				if (res.code === 0) {
					this.backgroundImage = res.data[0].imageUrl;
				}
			});

			this.$Request.getT('/app/common/type/116').then(res => {
				if (res.code === 0) {
					if (res.data && res.data.value) {
						this.tuiguang = res.data.value;
					}
				}
			});

			// #ifdef MP-WEIXIN
			if (e.scene) {
				const scene = decodeURIComponent(e.scene);
				this.$queue.setData('inviterCode', scene.split(',')[0]);
			}
			// #endif
			// 获取邀请码保存到本地
			if (e.invitation) {
				that.$queue.setData('inviterCode', e.invitation);
			}

			that.$Request.getT('/app/common/type/278').then(res => { //报名成功通知
				if (res.code == 0) {
					if (res.data && res.data.value) {
						that.arr.push(res.data.value)
					}
				}
			})
			that.$Request.getT('/app/common/type/310').then(res => { //新人领取优惠劵Id
				if (res.code == 0) {
					if (res.data && res.data.value) {
						let coupon = res.data.value.split(',')
						that.couponId = coupon[0]
						that.couponNum = coupon[1]
						// console.log(that.couponId,that.couponNum)
					}
				}
			})
			
			
			that.$Request.getT('/app/common/type/416').then(res => { // 否开启定位	416
				if (res.code == 0) {
					if (res.data && res.data.value && res.data.value=='是') {
						uni.getLocation({
							type: 'gcj02',
							geocode: true, //设置该参数为true可直接获取经纬度及城市信息
							success: function(res) {
								console.log(res, '地理位置')
								that.latitude = res.latitude
								that.longitude = res.longitude
								uni.setStorageSync('latitude', res.latitude)
								uni.setStorageSync('longitude', res.longitude)
								that.getzhandian(that.longitude, that.latitude)
							
							},
							fail: function() {
								console.log('获取地址失败')
							}
						})
					}
				}
			})
			
		},

		onShow() {
			let that = this
			that.isOrder = that.$queue.getData('isOrder');
			that.newYouhui = that.$queue.getData('newYouhui');
			that.XCXIsSelect = that.$queue.getData('XCXIsSelect');
			that.Qe = uni.getStorageSync("Qe")
			that.geRen = uni.getStorageSync("geRen")
			that.isZhandian = uni.getStorageSync("isZhandian")?uni.getStorageSync("isZhandian"):'否'
			if (uni.getStorageSync("sitlaundryName")) {
				that.laundryName = uni.getStorageSync("sitlaundryName")
				that.laundryId = uni.getStorageSync("sitlaundryId")
			} else {
				that.laundryName = uni.getStorageSync("sitlaundryName")
				that.laundryId = uni.getStorageSync("sitlaundryId")
			}


			if (that.Qe == 2 || that.geRen == 2) {
				this.renzheng = false
			} else {
				this.renzheng = true
			}
			that.city = uni.getStorageSync('city') ? uni.getStorageSync('city') : '请选择城市'
			that.page = 1
			that.getorderList()
			that.getBannerList()
			that.getGrid()
			that.getNewOrder()

			that.token = uni.getStorageSync('token')
			if (uni.getStorageSync('token')) {
				that.getIsVip()
			}
			that.myId = uni.getStorageSync('userId')
			if (that.first && that.myId) {
				this.$Request.get('/app/common/type/316').then(res => { //是否开启新人优惠券配置  0不开启 1开启
					if (res.code == 0 && res.data.value == 1) {
						that.$Request.get("/app/user/selectUserById").then(res => {
							if (res.code == 0 && res.data) {
								if (res.data.isNewPeople == 1) {
									that.popushongbao = true
									that.first = false
								} else {
									that.first = true
								}
							}
						});
					}
				});
			}
			// console.log(that.showModal, '------', that.myId, that.first)
			// #ifdef MP-WEIXIN
			//订阅
			if (that.myId) {
				if (that.showModal) {
					that.openMsg()
				}
			}
			// #endif

		},
		methods: {
			goZhandian() {
				if (this.token) {
					// #ifdef MP-WEIXIN
					if (uni.getStorageSync('sendMsg')) {
						console.log('授权+1')
						wx.requestSubscribeMessage({
							tmplIds: this.arr,
							success(re) {
								console.log(JSON.stringify(re), 111111111111)
								var datas = JSON.stringify(re);
								if (datas.indexOf("accept") != -1) {
									console.log(re)
								}
							},
							fail: (res) => {
								console.log(res)
							}
						})
					}
					// #endif
					uni.navigateTo({
						url: '/my/address/zhandian'
					})
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					});
				}
			},
			getzhandian(longitude, latitude) {
				this.$Request.get("/app/laundry/selectLaundryByDistance", {
					latitude: this.latitude,
					longitude: this.longitude
				}).then(res => {
					if (res.status == 0) {
						// this.laundry = res.data
						if (res.data) {
							this.laundryName = res.data.laundry_name;
							this.laundryId = res.data.laundry_id;
							this.$queue.setData('sitlaundryName', res.data.laundry_name)
							this.$queue.setData('sitlaundryId', res.data.laundry_id)
							// if (res.data.distance <= res.data.max_scope) {
							// 	this.laundryName = res.data.laundry_name;
							// 	this.laundryId = res.data.laundry_id;
							// 	this.$queue.setData('sitlaundryName',  res.data.laundry_name)
							// 	this.$queue.setData('sitlaundryId', res.data.laundry_id)
							// } else {
							// 	console.log('888')
							// 	this.laundryName = '';
							// 	this.laundryId = '';
							// 	// this.$queue.showToast('当前附近' + res.data.max_scope + '米内没有师傅可以服务！');
							// }
						}
						// else {
						// 	this.$queue.showToast('当前附近没有师傅可以服务！');
						// 	this.laundryName = '';
						// 	this.laundryId = '';
						// }
					} else {
						uni.showToast({
							title: res.msg,
							duration: 1000,
							icon: 'none'
						});
					}
				});
			},
			GetQuan() {
				let data = {
					couponId: this.couponId,
					num: this.couponNum
				}
				this.$Request.get('/app/couponUser/receiveEnvelope', data).then(res => {
					if (res.code == 0) {
						this.first = false
						this.popushongbao = false
						uni.showToast({
							title: '领取成功',
							icon: 'none',
						})
						setTimeout(d => {
							uni.navigateTo({
								url: '/my/youhuijuan/myList'
							});
						}, 1000);
					}
				});
			},
			gosearch() {
				uni.navigateTo({
					url: '/pages/index/search/index'
				})
			},
			// 筛选
			fterChange(id) {
				this.defaultIndex = id
				this.page = 1
				this.getorderList()
			},
			getNewOrder() {
				this.$Request.get('/app/orders/selectNewestOrders').then(res => {
					if (res.code == 0) {
						this.messageList = res.data
					}
				});

			},
			selectCity(longitude, latitude) {
				this.$Request.get('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					if (res.code == 0) {
						this.city = res.data.city ? res.data.city : '未知'
						uni.setStorageSync('city', res.data.city)
						this.page = 1
						this.getorderList()
					}
				});
			},
			// 开启订阅消息
			openMsg() {
				console.log('订阅消息')
				var that = this
				uni.getSetting({
					withSubscriptions: true, //是否获取用户订阅消息的订阅状态，默认false不返回
					success(ret) {
						console.log(ret.subscriptionsSetting, '------------------')
						// if (ret.subscriptionsSetting.itemSettings && Object.keys(ret.subscriptionsSetting.itemSettings).length == 2) {
						if (ret.subscriptionsSetting.itemSettings) {
							uni.setStorageSync('sendMsg', true)
							uni.openSetting({ // 打开设置页 
								success(rea) {
									console.log(rea.authSetting)
								}
							});
						} else { // 用户没有点击“总是保持以上，不再询问”则每次都会调起订阅消息
							console.log(99999)
							uni.setStorageSync('sendMsg', false)
							uni.showModal({
								title: '提示',
								content: '为了更好的体验,请绑定消息推送',
								confirmText: '确定',
								cancelText: '取消',
								success: function(res) {
									if (res.confirm) {
										wx.requestSubscribeMessage({
											tmplIds: that.arr,
											success(re) {
												console.log(JSON.stringify(re),
													'++++++++++++++')
												var datas = JSON.stringify(re);
												if (datas.indexOf("accept") != -1) {
													console.log(re)
													// uni.setStorageSync('sendMsg', true)
												}
											},
											fail: (res) => {
												console.log(res)
											}
										})
										// uni.setStorageSync('sendMsg', true)
										console.log('确认')
										that.showModal = false
									} else if (res.cancel) {
										console.log('取消')
										// uni.setStorageSync('sendMsg', false)
										that.showModal = true
									}
								}
							})
						}
					}
				})
			},
			getIsVip() {
				this.$Request.get("/app/UserVip/isUserVip").then(res => {
					if (res.code == 0) {
						this.isVip = res.data
						uni.setStorageSync('isVIP', res.data)
					}
				});
			},
			getorderList() {
				this.$Request.get("/app/appGame/queryGameNameList").then(res => {
					if (res.code == 0) {
						this.List = res.data
						this.loading = false;
					}

				})

			},
			// 切换菜单
			tabChange() {
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								// console.log(re)
							}
						},
						fail: (res) => {
							// console.log(res)
						}
					})
				}
				// this.defaultIndex = [0, 0, 0]
			},
			// 获取游戏类型
			getClassfly() {
				this.$Request.get("/app/appGame/queryGameName").then(res => {
					if (res.code == 0) {
						this.tabData = [{
							createTime: "",
							gameName: '服务推荐',
							gameImg: "",
							id: 0,
							status: 0,
							updateTime: "",
						}]
						this.tabData = [...this.tabData, ...res.data]

					}
				});
			},
			//获取轮播图
			getBannerList() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 1
				}).then(res => {
					if (res.code == 0) {
						this.swiperList = res.data
					}
				});
			},
			// 获取金刚区分类
			getGrid() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 2
				}).then(res => {
					if (res.code == 0) {
						this.gridData = res.data
						// console.log(this.gridData, '；；；；；；')
					}
				});
			},

			// 筛选
			change(e) {
				this.value1 = e[0][0].value
				this.value2 = e[1][0].value
				this.value3 = e[2][0].value

				this.mescroll.resetUpScroll()
			},
			// 选择城市
			goSelectCity() {
				uni.navigateTo({
					url: '/pages/index/citys/citys'
				});
			},

			// 跳转游戏列表
			goNav(url) {
				console.log(url, '1111112333')
				// #ifdef MP-WEIXIN
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				// #endif
				if (url.indexOf('/pages/') !== -1||url.indexOf('/my/') !== -1) {
					uni.navigateTo({
						url
					});
				} else {
					//#ifndef H5
					uni.navigateTo({
						url: '/pages/index/webView?url=' + url
					});
					//#endif
					//#ifdef H5
					window.location.href = url;
					//#endif
				}
			},
			goOrdeDet() {
				if (this.token) {
					// #ifdef MP-WEIXIN
					if (uni.getStorageSync('sendMsg')) {
						console.log('授权+1')
						wx.requestSubscribeMessage({
							tmplIds: this.arr,
							success(re) {
								console.log(JSON.stringify(re), 111111111111)
								var datas = JSON.stringify(re);
								if (datas.indexOf("accept") != -1) {
									console.log(re)
								}
							},
							fail: (res) => {
								console.log(res)
							}
						})
					}
					// #endif

					uni.navigateTo({
						url: '/my/order/orderDet'
					});
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					});
				}

			},
			// 跳转搜索
			goSearch() {
				// #ifdef MP-WEIXIN
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				// #endif
				uni.navigateTo({
					url: '/pages/index/search/index'
				});
			},
			// 跳转订单
			clickItem(item, index) {
				if (this.token) {
					// #ifdef MP-WEIXIN
					if (uni.getStorageSync('sendMsg')) {
						console.log('授权+1')
						wx.requestSubscribeMessage({
							tmplIds: this.arr,
							success(re) {
								console.log(JSON.stringify(re), 111111111111)
								var datas = JSON.stringify(re);
								if (datas.indexOf("accept") != -1) {
									console.log(re)
								}
							},
							fail: (res) => {
								console.log(res)
							}
						})
					}
					// #endif
					uni.navigateTo({
						url: '/pages/index/game/order?id=' + item.id + '&titleName=' + item.serviceName
					});
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					});
				}
			},
		},
		onReachBottom: function() {
			this.page = this.page + 1;
			this.getorderList()
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.getorderList()
		},
	}
</script>

<style lang="scss">
	.u-mode-center-box {
		background-color: '' !important;
	}

	page {
		background-color: #F4F8FB;
	}

	.bg {
		background: #FFFFFF;
	}

	.zhandian {
		display: flex;
		align-items: center;
		justify-content: space-between;
		background-color: #FFFFFF;
		padding: 15rpx 30rpx;
		position: fixed;
		/* #ifdef H5 */
		top: 85rpx;
		/* #endif */
		/* #ifndef H5 */
		top: 0;
		/* #endif */
		left: 0;
		right: 0;
		z-index: 999;
	}

	.fiterBox {
		background: #FFFFFF;
		border-radius: 8upx;
		padding: 10upx 18upx;
	}

	.active {
		background: #E5F0FF;
		color: #005DFF;
	}

	.fw {
		width: 100%;
		// height: 180rpx;
		background-color: #F4F8FB;
		display: flex;
		justify-content: center;
		align-items: flex-end;
		/* #ifdef H5 */
		margin-top: 60rpx;
		/* #endif */
		/* #ifndef H5 */
		margin-top: 60rpx;
		/* #endif */

		.fw-box {
			width: 100%;
			margin: 30upx;
			// height: 160rpx;
			// background-color: #ffffff;
			border-radius: 24rpx;
		}
	}

	.sticky-tabs {
		z-index: 990;
		position: sticky;
		top: var(--window-top);
		// background-color: #fff;
	}

	/* // 使用mescroll-uni,则top为0 */
	.mescroll-uni,
	/deep/.mescroll-uni {
		.sticky-tabs {
			top: 0;
		}
	}

	.demo-tip {
		padding: 18upx;
		font-size: 24upx;
		text-align: center;
	}

	.line_s {
		display: inline-flex;
		width: 10rpx;
		height: 10rpx;
		background: #1AD566;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.line_x {
		display: inline-flex;
		width: 10rpx;
		height: 10rpx;
		background: #000000;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.index_gonggao {
		color: #FDFFFE;
		width: 550rpx;
		height: 68rpx;
		// background: #EAF4FE;
		border-radius: 50rpx;
		align-items: center;
		line-height: 50rpx;
		padding: 10rpx 15rpx;

		text {
			height: 80rpx;
			font-size: 26rpx;
			font-family: PingFang SC Medium, PingFang SC Medium-Medium;
			font-weight: 500;
			margin-left: 10rpx;
			color: #666666;
		}
	}

	.listbox {
		background: #FFFFFF;
		border-radius: 16upx;
	}

	.listbox image {
		width: 333upx;
		height: 333upx;
		background: #FFFFFF;
		border-radius: 16upx 16upx 0 0;
		// border-radius: 16upx;
	}

	.labels {
		font-size: 24upx;
		padding: 0upx 10upx;
		height: 38upx;
		line-height: 38upx;
		border: 1upx solid #025EFD;
		color: #025EFD;
		margin-left: 20upx;
	}

	.heafbot {
		position: fixed;
		bottom: 150rpx;
		right: 30rpx;

		image {
			width: 214rpx;
			height: 88rpx;
		}
	}
</style>