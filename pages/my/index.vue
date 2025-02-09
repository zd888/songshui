<template>
	<view class="text-white ">
		<view style="background: linear-gradient(0deg, #F2F6FC 0%, #E5EEFF 51%);">
			<view class=" u-flex u-p-l-30 u-p-t-30 u-p-b-30">
				<view class="u-m-r-10">
					<image :src="avatar" style="width: 100rpx;height: 100rpx;border-radius: 100rpx;"
						@click="goNav('/pages/my/userinfo')"></image>
					<!-- <view class="lovip" v-if="isVip == true&& XCXIsSelect != '否'&&!isLogin">
						<image src="../../static/images/my/vip.png" style="width: 100%;height: 100%;"></image>
					</view> -->
				</view>
				<view class="u-flex-1 u-m-l-10 text-white" v-if="!isLogin">
					<view class="u-font-18  text-bold">
						<view class="flex align-center ">
							<view class=" " @click="goNav('/pages/my/userinfo')">{{userName}}</view>
						</view>
						<view class=" text-sm">ID:{{invitationCode}}</view>
					</view>
					<!-- 	<view class="u-font-14 u-tips-color" style="color: #000000;font-size: 24rpx;margin-top: 5rpx;"
					@click="goNav('/pages/me/vip/index')">邀请码:{{invitationCode}}</view> -->
				</view>
				<view v-else class="text-xl u-p-l-20 text-bold" @click="goLogin('/pages/public/login')">
					登录
				</view>
				<!-- <view v-if="!isLogin" class="" @click="goNav('/pages/my/invitationUser')">
				<image src="../../static/images/my/yaoqing.png" style="width: 163rpx;height: 58rpx;" mode=""></image>
			</view> -->
			</view>
			<view class="flex justify-around tanbox1" v-if="XCXIsSelect != '否'">
				<!-- <view class="text-26 text-center" @click="goNav('/my/jifen/jifen')">
					<view class="text-38 text-bold">{{fans}}</view>
					我的积分
				</view> -->
				<view class="text-26  flex-sub text-center" @click="goNav('/my/wallet/wallet')" v-if="isShuibei =='是'">
					我的水贝
					<view class="text-38 text-bold margin-top-xs">{{money}}</view>
				</view>
				<view class="text-26  flex-sub text-center" @click="goNav('/my/shuipiao/index?index=1')" v-else>
					我的水票
					<view class="text-38 text-bold margin-top-xs">{{shuipiao}}</view>
				</view>
				<view style="width: 1rpx;height: 80upx;background: #f5f5f5;"></view>
				<view class="text-26 text-center flex-sub" @click="goNav('/my/youhuijuan/index')">
					优惠劵
					<view class="text-38 text-bold margin-top-xs">{{youhuiNumber}}</view>
				</view>
				<view style="width: 1rpx;height: 80upx;background: #f5f5f5;"></view>
				<view class="text-26  flex-sub text-center" @click="goNav('/my/gird/browse')">
					我的收藏
					<view class="text-38 text-bold margin-top-xs">{{browse}}</view>
				</view>
				<view style="width: 1rpx;height: 80upx;background: #f5f5f5;"></view>
				<!-- <view class="text-26  flex-sub text-center" @click="goNav('/my/shuipiao/index?index=1')">
					我的水票
					<view class="text-38 text-bold margin-top-xs">{{myYouhuilist}}</view>
				</view> -->
				<!-- @click="openTong" -->
				<view class="text-26  flex-sub text-center" @click="goNav('/my/tong/index')">
					我的押桶
					<view class="text-38 text-bold margin-top-xs">{{bucket}}</view>
				</view>
			</view>


			<view class="tanbox bg  padding-lr padding-tb">
				<view class="flex align-center justify-between">
					<view>我的订单</view>
					<view class="flex align-center" @click="goNav('/my/order/index?index=0')">查看全部
						<image src="../../static/images/my/right.png"
							style="width: 15upx;height: 22upx;margin-left: 8upx;"></image>
					</view>
				</view>
				<view class="flex align-center justify-between margin-top">
					<view class=" text-center " @click="goNav('/my/order/index?index=1')" style="position: relative;">
						<image src="../../static/images/my/daifukuan.png" style="width: 46upx;height: 38upx;"></image>
						<view class="text-sm margin-top-xs">待付款</view>
						<view class="borderRa" v-if="count0">{{count0}}</view>
					</view>
					<view class=" text-center " @click="goNav('/my/order/index?index=2')" style="position: relative;">
						<image src="../../static/images/my/daishouhuo.png" style="width: 38upx;height: 46upx;"></image>
						<view class="text-sm margin-top-xs">待接单</view>
						<view class="borderRa" v-if="count4">{{count4}}</view>
					</view>
					<view class="  text-center" @click="goNav('/my/order/index?index=4')" style="position: relative;">
						<image src="../../static/images/my/tuihuna.png" style="width: 46upx;height: 40upx;"></image>
						<view class="text-sm margin-top-xs">进行中</view>
						<view class="borderRa" v-if="count1">{{count1}}</view>
					</view>
					<view class="  text-center" @click="goNav('/my/order/index?index=5')">
						<image src="../../static/images/my/all.png" style="width: 39upx;height: 46upx;"></image>
						<view class="text-sm margin-top-xs">已完成</view>
					</view>
				</view>
			</view>


			<view class="margin-lr flex align-center justify-between" v-if="XCXIsSelect != '否'">
				<view class="flex align-center justify-between" style="position: relative;color: #fff;"
					@click="applyhome()">
					<image src="../../static/images/my/daisang.png" style="width: 333upx;height: 160upx;"></image>
					<view style="position: absolute;left:15rpx;">
						<view class="text-lg text-bold">代理商中心</view>
						<view class="text-sm margin-top-xs">申请成为代理商</view>
					</view>
				</view>
				<view class="flex align-center justify-between" style="position: relative;color: #fff;"
					@click="tuiguang()">
					<image src="../../static/images/my/tuiguang.png" style="width: 333upx;height: 160upx;"></image>
					<view style="position: absolute;left:15rpx;">
						<view class="text-lg text-bold">推广中心</view>
						<view class="text-sm margin-top-xs">进入立即推广</view>
					</view>
				</view>
			</view>

		</view>


		<view class="tanbox padding-tb">
			<view class="padding-lr">推荐工具</view>
			<view class="flex flex-wrap margin-top">
				<view class="image_tu" @click="openTong" v-if="XCXIsSelect != '否'&&isTong=='否'">
					<image src="../../static/images/my/tong.png" style="width: 50upx;height: 50upx;"></image>
					<view>我要押桶</view>
				</view>
				<view class="image_tu" @click="goNav('/my/shuipiao/index?index=0') " v-if="XCXIsSelect != '否'">
					<image src="../../static/images/my/shuipiao.png" style="width: 50upx;height: 50upx;"></image>
					<view>水票中心</view>
				</view>
				<view class="image_tu" @click="goNav('/my/order/tousuList') " v-if="XCXIsSelect != '否'">
					<image src="../../static/images/my/tousu.png" style="width: 50upx;height: 50upx;"></image>
					<view>投诉记录</view>
				</view>
				<view class="image_tu" @click="goNav('/pages/my/invitationUser')">
					<image src="../../static/images/my/yaoqing.png" style="width: 50upx;height: 48upx;"></image>
					<view>邀请好友</view>
				</view>
				<view class="image_tu" @click="goNav('/my/other/cooperation')" v-if="XCXIsSelect != '否'&&isAgent!=1">
					<image src="../../static/images/my/jiameng.png" style="width: 50upx;height: 50upx;"></image>
					<view>招商加盟</view>
				</view>
				<view class="image_tu" @click="goNav('/pages/msg/message')">
					<image src="../../static/images/my/msg.png" style="width: 50upx;height: 48upx;"></image>
					<view>系统消息</view>
				</view>
				<view class="image_tu" @click="goNav('/my/address/address')" v-if="XCXIsSelect != '否'">
					<image src="../../static/images/my/address.png" style="width: 44upx;height: 50upx;"></image>
					<view>我的地址</view>
				</view>
				<view class="image_tu" @click="Changekefu()">
					<image src="../../static/images/my/songshuyuan.png" style="width: 41upx;height: 49upx;"></image>
					<view>联系客服</view>
				</view>

				<view class="image_tu" @click="goNav('/my/feedback/index')">
					<image src="../../static/images/my/yijian.png" style="width: 50upx;height: 48upx;"></image>
					<view>意见反馈</view>
				</view>

				<view class="image_tu" @click="goNav('/my/feedbackIndex/feedbackIndex')" v-if="XCXIsSelect != '否'">
					<image src="../../static/images/my/help.png" style="width: 50upx;height: 48upx;"></image>
					<view>帮助中心</view>
				</view>

				<view class="image_tu" @click="goNav('/my/setting/index')">
					<image src="../../static/images/my/set.png" style="width: 50upx;height: 48upx;"></image>
					<view>设置中心</view>
				</view>
			</view>
		</view>

		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
				number: '',
				customStyle: {
					background: '#F5F5F5'
				},

				avatar: '../../static/logo.png',
				isLogin: true,
				userName: '匿名',
				browse: 0, //浏览数
				fans: 0, //粉丝数
				follow: 0, //关注数
				visitor: 0, //访客数
				userId: '',
				isVip: false,
				invitationCode: '', //邀请码

				geRen: 0,
				Qe: 0,
				XCXIsSelect: '否',
				renzheng: false,
				money: 0, //余额
				youhuiNumber: 0, //优惠卷
				myYouhuilist: 0,
				jifen: 0, //我的积分
				count0: 0,
				count4: 0,
				count1: 0,
				isPromotion: '', //推广员  1是  0或者null 不是
				isAgent: '', //代理商  1是  0或者null 不是
				
				bucket: 0,
				isTong: '否',
				
				isShuibei: '是',
				shuipiao: 0,
			}
		},
		onLoad() {
			this.XCXIsSelect = this.$queue.getData("XCXIsSelect");
			

		},
		onShow() {
			this.isTong = this.$queue.getData('isTong');
			this.orderCount = uni.getStorageSync('orderCount')
			
			this.isShuibei = this.$queue.getData('isShuibei');
			
			this.Qe = uni.getStorageSync("Qe")
			this.geRen = uni.getStorageSync("geRen")
			if (this.Qe == 2 || this.geRen == 2) {
				this.renzheng = false
			} else {
				this.renzheng = true
			}
			this.userId = uni.getStorageSync('userId')
			if (this.userId) {
				this.isLogin = false
				this.getUserInfo()
				this.getRenZheng()
				this.getRenZhengs()
				this.getBrowseList()
				this.getIsVip()
				this.getMyMoney()
				this.getMyjifen()
				this.getShuipiao()
				this.getorder1()
				this.getorder2()
				this.getorder3()
				this.getMyList()
			} else {
				this.isLogin = true
				this.userName = '匿名'
				this.browse = 0
				this.fans = 0
				this.follow = 0
				this.visitor = 0
				this.money = 0
				this.youhuiNumber = 0
				this.myYouhuilist = 0
				this.count0 = 0
				this.count4 = 0
				this.count1 = 0
				this.shuipiao = 0
				this.avatar = '../../static/logo.png'
			}

		},
		methods: {
			getShuipiao() { //我的水票
				this.$Request.get("/app/tickets/getUserTicketNum").then(res => { //获取水票总数量
					if (res.code == 0) {
						this.shuipiao = res.data ? res.data : '0'
					} else {
						this.shuipiao = 0
						console.log(res.msg)
					}
				})
			},
			selectWay: function(item) {
				this.openWay = item.id;
			},
			// 我的优惠卷
			getMyList() {
				this.$Request.getT('/app/couponUser/getMyCouponList?status=0').then(res => {
					if (res.code == 0) {
						this.youhuiNumber = res.data.records.length ? res.data.records.length : 0
					}
				});

				this.$Request.get("/app/tickets/getUserTicketNum").then(res => {
					if (res.code == 0) {
						this.myYouhuilist = res.data ? res.data : 0
					}

				})
			},
			getBrowseList() {
				let data = {
					page: 1,
					limit: 1
				}
				this.$Request.get("/app/collect/selectCollectOrderTakingList", data).then(res => {
					uni.hideLoading();
					if (res.code == 0) {
						this.browse = res.data.totalCount ? res.data.totalCount : 0
					}
				})
			},
			applyhome() {
				if (this.userId) {
					if (this.isAgent == 0) {
						uni.navigateTo({
							url: '/my/other/cooperation'
						})
					} else {
						uni.navigateTo({
							url: '/my/other/agent',
						})
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
			tuiguang() {
				if (this.userId) {
					if (this.isPromotion == 0) {
						uni.navigateTo({
							url: '/my/other/team'
						})
					} else {
						uni.navigateTo({
							url: '/my/team/team',
						})
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
			//订单未读角标
			getorder1() {
				let data = {
					status: 0,
					page: 1,
					limit: 1
				}
				this.$Request.get('/app/orders/selectMyOrder', data).then(res => {
					if (res.code == 0) {
						this.count0 = res.data.totalCount
					}
				})
			},
			getorder2() {
				let data = {
					status: 4,
					page: 1,
					limit: 1
				}
				this.$Request.get('/app/orders/selectMyOrder', data).then(res => {
					if (res.code == 0) {
						this.count4 = res.data.totalCount
					}
				})
			},
			getorder3() {
				let data = {
					status: 1,
					page: 1,
					limit: 1
				}
				this.$Request.get('/app/orders/selectMyOrder', data).then(res => {
					if (res.code == 0) {
						this.count1 = res.data.totalCount
					}
				})
			},
			//我的余额
			getMyMoney() {
				this.$Request.get("/app/userMoney/selectMyMoney").then(res => {
					if (res.code == 0) {
						this.money = res.data.money
					}
				})
			},
			//我的积分
			getMyjifen() {
				this.$Request.get("/app/userintegral/selectUserIntegral").then(res => {
					if (res.code == 0) {
						this.jifen = res.data.integralNum
					}
				})
			},
			Changekefu() {
				let that = this
				let kefu = this.$queue.getData('kefu'); // 用户端联系方式 1 手机号 2企业微信 3 客服二维码
				let kefuPhone = this.$queue.getData('kefuPhone');
				if (kefu == 1) {
					uni.makePhoneCall({
						phoneNumber: kefuPhone //仅为示例
					});
				} else if (kefu == 3) { //客服二维码
					uni.navigateTo({
						url: '/my/setting/customer'
					});
				} else {
					// #ifdef MP-WEIXIN
					let that = this
					try {
						wx.openCustomerServiceChat({
							extInfo: {
								url: that.$queue.getData('kefuUrl')
							},
							corpId: that.$queue.getData('kefuAppId'),
							success(res) {},
							fail(res) {
								console.error(res)
							}
						})
					} catch (error) {
						console.error("catchcatch" + error)
						uni.showToast({
							title: '请更新至微信最新版本'
						});
					}
					// #endif
					// #ifndef MP-WEIXIN
					let url = this.$queue.getData('kefuUrl');
					if (url.indexOf('/pages/') !== -1 || url.indexOf('/my/') !== -1) {
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
					// #endif
				}
			},
			goNav(e, name) {
				if (this.userId) {
					uni.navigateTo({
						url: e
					})

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
			
			goNavs(e, name) {
				console.log(e)
				if (this.userId) {
					uni.switchTab({
						url: e
					})

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
			goLogin(e) {
				uni.navigateTo({
					url: e
				})
			},
			getAmount() {
				this.$Request.get("/app/userBrowse/selectAmount").then(res => {
					if (res.code == 0) {
						this.browse = res.data.browse
						this.fans = res.data.fans
						this.follow = res.data.follow
						this.visitor = res.data.visitor
					}
				});
			},
			getUserInfo() {
				this.$Request.get("/app/user/selectUserById").then(res => {
					if (res.code == 0) {
						this.userName = res.data.userName
						this.invitationCode = res.data.invitationCode
						this.avatar = res.data.avatar ? res.data.avatar : '../../static/logo.png'
						this.isAuthentication = res.data.isAuthentication
						this.bucket = res.data.bucket
						if (res.data.isPromotion == null || res.data.isPromotion == 0) { //推广员
							this.isPromotion = '0'
						} else {
							this.isPromotion = res.data.isPromotion
						}
						if (res.data.isAgent == null || res.data.isAgent == 0) { //代理商
							this.isAgent = '0'
						} else {
							this.isAgent = res.data.isAgent
						}

						uni.setStorageSync('isAuthentication', res.data.isAuthentication)

						uni.setStorageSync('avatar', res.data.avatar)
						uni.setStorageSync('invitationCode', res.data.invitationCode)
						uni.setStorageSync('zhiFuBao', res.data.zhiFuBao)
						uni.setStorageSync('zhiFuBaoName', res.data.zhiFuBaoName)

					}
				});
			},
			// 个人认证数据
			getRenZheng() {
				let classify
				this.$Request.get("/app/userCertification/queryInsert?classify=" + 1).then(res => {
					console.log(res)
					if (res.code == 0) {
						if (res.data == null) {
							this.geRen = 0
							uni.setStorageSync("geRen", this.geRen)
						} else if (res.data.status == 0) {
							this.geRen = 1
							uni.setStorageSync("geRen", this.geRen)
						} else if (res.data.status == 1) {
							this.geRen = 2
							uni.setStorageSync("geRen", this.geRen)
						} else if (res.data.status == 2) {
							this.geRen = 3
							uni.setStorageSync("geRen", this.geRen)
						}
					}
				});
			},
			// 企业认证数据
			getRenZhengs() {
				let classify
				this.$Request.get("/app/userCertification/queryInsert?classify=" + 2).then(res => {
					console.log(res)
					if (res.code == 0) {
						if (res.data == null) { //未实名
							this.Qe = 0
							uni.setStorageSync("Qe", this.Qe)
						} else if (res.data.status == 0) { //审核中
							this.Qe = 1
							uni.setStorageSync("Qe", this.Qe)
						} else if (res.data.status == 1) { //已实名
							this.Qe = 2
							uni.setStorageSync("Qe", this.Qe)
						} else if (res.data.status == 2) { //已拒绝
							this.Qe = 3
							uni.setStorageSync("Qe", this.Qe)
						}
					}
				});
			},
			getIsVip() {
				this.$Request.get("/app/UserVip/isUserVip").then(res => {
					if (res.code == 0) {
						this.isVip = res.data
						uni.setStorageSync('isVIP', res.data)
					}
				});
			},
			
		}
	}
</script>

<style lang="scss">
	page {
		background: #F2F2F7;
	}

	.bg {
		background: #FFFFFF;
	}

	button::after {
		border: none;
		background-color: none;
	}

	button {
		position: relative;
		display: block;
		margin-left: auto;
		margin-right: auto;
		padding-left: 0px;
		padding-right: 0px;
		box-sizing: border-box;
		text-decoration: none;
		line-height: 1.35;
		overflow: hidden;
		color: #666666;
		/* background-color: #fff; */
		background-color: rgba(255, 255, 255, 0) !important;
		width: 100%;
		height: 100%;
	}

	.camera {
		width: 54px;
		height: 44px;

		&:active {
			background-color: #ededed;
		}
	}

	.btn-bg {
		width: 64px;
		height: 28px;
		background: linear-gradient(90deg, #CDA26E 0%, #DCB78A 100%);
		border-radius: 28px;
		text-align: center;
		line-height: 28px;
		margin-top: 4px;
		color: '#604320'
	}

	.images {
		width: 18rpx;
		height: 30rpx;
	}

	.dfs {
		display: flex;
		align-items: center;
	}

	.lovip {
		width: 100rpx;
		height: 35rpx;
		position: relative;
		top: -39rpx;
	}

	.box {
		width: 48%;
		border-radius: 16upx;
		display: flex;
		align-items: center;
		background: #FFFFFF;
		padding: 30upx;
	}

	.tanbox1 {
		background: #FFFFFF;
		border-radius: 24upx;
		margin: 30upx;
		padding: 30upx;
		display: flex;
		text-align: left;
	}

	.tanbox {
		background: #FFFFFF;
		border-radius: 24upx;
		margin: 30upx;

	}

	.image_tu {
		text-align: center;
		width: 25%;
		margin-bottom: 30upx;
		font-size: 26upx;
	}

	.borderRa {
		position: absolute;
		top: -10rpx;
		right: -10rpx;
		background: red;
		border-radius: 50rpx;
		/* padding: 2rpx 10rpx; */
		width: 40rpx;
		// padding: 5upx;
		text-align: center;
		color: #FFFFFF;

	}

</style>