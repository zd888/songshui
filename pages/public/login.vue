<template>
	<view class="container">
		<!-- <image @click="navBack" src="../../static/images/index/close.png" style="width: 32upx;height: 32upx;margin-left: 46upx;"></image> -->
		<!-- 小程序状态下登录 -->
		<!-- #ifdef MP-WEIXIN -->
		<view class="mp_wxBox">
			<view>
				<view class="headers">
					<image src="../../static/logo.png" style="border-radius: 50%;"></image>
				</view>
				<view class="content">
					<view>申请获取以下权限</view>
					<text>获得你的公开信息(昵称，头像、地区等)</text>
				</view>
				<button v-show="weixinPhone" style="background: #557EFD;color: #FFFFFF;" class="bottom"
					open-type="getPhoneNumber" @getphonenumber="getPhoneNumber">
					授权手机号
				</button>
				<button v-show="!weixinPhone" style="background: #557EFD;color: #FFFFFF;" class='bottom'
					bindtap="getUserProfile" @tap="wxGetUserInfo">
					授权登录
				</button>
			</view>
		</view>
		<view class="footer">
			<image v-if="showAgree" @tap="isShowAgree"
				src="https://api.shengqianxiong.com.cn/img/20201112/669aa8946cfb4ebdb459d57193c0ebca.png"
				style="width: 34upx;height: 34upx;"></image>
			<image v-else @tap="isShowAgree"
				src="https://api.shengqianxiong.com.cn/img/20201112/1e9102fc891f4d86a13c7b2ba6921cba.png"
				style="width: 34upx;height: 34upx;"></image>
			<text class="margin-left-xs">同意</text>
			<!-- 协议地址 -->
			<navigator url="/my/setting/mimi" open-type="navigate">《隐私政策》</navigator>
			和
			<navigator url="/my/setting/xieyi" open-type="navigate">《用户服务协议》</navigator>
		</view>
		<!-- #endif -->
		<!-- #ifndef MP-WEIXIN -->
		<view style="text-align: center;">
			<image style="width: 120upx;height: 120upx;margin-top: 140upx;border-radius:20upx"
				src="../../static/logo.png"></image>
			<view style="font-size: 28upx;margin-top: 32upx">欢迎使用省钱兄家政系统</view>

			<button v-if="weixinLogin" class="confirm-btn" @click="weixinLo">微信登录</button>
			<button v-if="weixinLogin" class='confirm-btn-weixin' @click="register">手机号登录</button>
			<button v-if="!weixinLogin" class='confirm-btn' @click="register">手机号登录</button>

			<!-- 底部信息 -->
			<view class="footer">
				<image v-if="showAgree" @tap="isShowAgree"
					src="https://api.shengqianxiong.com.cn/img/20201112/669aa8946cfb4ebdb459d57193c0ebca.png"
					style="width: 34upx;height: 34upx;"></image>
				<image v-else @tap="isShowAgree"
					src="https://api.shengqianxiong.com.cn/img/20201112/1e9102fc891f4d86a13c7b2ba6921cba.png"
					style="width: 34upx;height: 34upx;"></image>
				<text class="margin-left-xs">同意</text>
				<!-- 协议地址 -->
				<navigator url="/my/setting/mimi" open-type="navigate">《隐私政策》</navigator>
				和
				<navigator url="/my/setting/xieyi" open-type="navigate">《用户服务协议》</navigator>
			</view>
		</view>
		<!-- #endif -->

	</view>
</template>
<script>
	export default {
		data() {
			return {
				mobile: '',
				code: '',
				weixinLogin: false,
				sending: false,
				sendTime: '获取验证码',
				count: 60,
				weixinPhone: false,
				sendDataList: {},
				sessionkey: '',
				phoneNum: false,
				showAgree: false,
			};
		},
		onLoad(e) {
			if (e.inviterCode) {
				this.$queue.setData('inviterCode', e.inviterCode);
			}
			//微信登录开启
			this.$Request.getT('/app/common/type/53').then(res => {
				if (res.code == 0) {
					if (res.data && res.data.value && res.data.value == '是') {
						// this.weixinLogin = true;
					}
				}
			});
			this.$Request.getT('/app/common/type/188').then(res => {
				if (res.code == 0) {
					if (res.data && res.data.value && res.data.value == '是') {
						this.phoneNum = true;
					}
				}
			});
		},
		methods: {
			isShowAgree() {
				//是否选择协议
				this.showAgree = !this.showAgree;
			},
			wxGetUserInfo(e) {
				if (this.showAgree == false) {
					uni.showToast({
						icon: 'none',
						position: 'bottom',
						title: '请先同意《用户协议》和《隐私政策》'
					});
					return false;
				}
				wx.getUserProfile({
					desc: '业务需要',
					success: infoRes => {
						console.log("infoRes.encryptedData__________:" + JSON.stringify(infoRes.userInfo))
						let nickName = infoRes.userInfo.nickName; //昵称
						let avatarUrl = infoRes.userInfo.avatarUrl; //头像
						let sex = infoRes.userInfo.gender; //头像
						try {
							this.$queue.showLoading('正在登录中...');
							this.login(nickName, avatarUrl, sex);
						} catch (e) {}
					}
				})
			},
			//登录
			login(nickName, avatarUrl, sex) {
				let that = this;
				// 1.wx获取登录用户code
				uni.login({
					provider: 'weixin',
					success: function(loginRes) {
						console.log(loginRes, '************')
						let data = {
							code: loginRes.code,
							
						}
						that.$Request.get('/app/Login/wxLogin', data).then(res => {
							if (res.code == 0) {
								uni.hideLoading()
								uni.setStorageSync('openId', res.data.open_id)
								uni.setStorageSync('unionId', res.data.unionId)
								that.sessionkey = res.data.session_key;

								let inviterCode = '';
								if (uni.getStorageSync('inviterCode')) {
									inviterCode = uni.getStorageSync('inviterCode')
								}
								let sendData = {
									openId: uni.getStorageSync('openId'),
									unionId: uni.getStorageSync('unionId'),
									userName: nickName,
									avatar: avatarUrl,
									sex: sex, //性别
									inviterCode: inviterCode //别人登录进来携带你的邀请码
								};
								that.sendDataList = sendData;
								that.flag = res.data.flag;
								// 第一次登录获取手机号
								console.log(that.phoneNum)
								if (that.flag == '1' && that.phoneNum) {
									that.weixinPhone = true;
								} else {
									that.getWeixinInfo(sendData);
								}
							} else {
								uni.showToast({
									icon: 'none',
									title: res.msg,
									duration: 2000
								});
								console.log(res, '失败')
							}
						})

					}
				});
			},
			//小程序微信登录后获取手机号
			getPhoneNumber: function(e) {
				if (e.detail.errMsg == 'getPhoneNumber:fail user deny') {
					console.log('用户拒绝提供手机号');
				} else {
					console.log('用户同意提供手机号');
					console.log(e)
					this.setPhoneByInsert(e.detail.encryptedData, e.detail.iv);
				}
			},
			//小程序微信登录后获取手机号
			setPhoneByInsert(decryptData, iv) {

				let data = {
					decryptData: decryptData,
					key: this.sessionkey,
					iv: iv
				};

				this.$Request.postJson('/app/Login/selectPhone', data).then(res => {
					if (res.code == 0) {
						this.phone = res.data.phoneNumber;
						this.getWeixinInfo(this.sendDataList);
					} else {
						uni.showToast({
							title: res.msg,
							icon: 'none',
							duration: 2000
						});
					}
				})
			},
			//获取个人信息
			getWeixinInfo(sendData) {
				let that = this;
				uni.showLoading({
					title: '登录中...'
				});
				let postData = {
					openId: sendData.openId, //小程序openId
					unionId: sendData.unionId, //unionId
					userName: sendData.userName, //微信名称
					avatar: sendData.avatar, //头像
					sex: sendData.sex, //性别
					phone: that.phone,
					inviterCode: sendData.inviterCode,
					
				};
				that.$Request.postJson('/app/Login/insertWxUser', postData).then(res => {
					uni.hideLoading();
					if (res.code == 0) {
						uni.setStorageSync('token', res.token)
						uni.setStorageSync('userName', res.user.userName)
						uni.setStorageSync('avatar', res.user.avatar)
						uni.setStorageSync('phone', res.user.phone)
						uni.setStorageSync('invitationCode', res.user.invitationCode)
						uni.setStorageSync('sex', res.user.sex)
						uni.setStorageSync('userId', res.user.userId)
						uni.setStorageSync('openId', res.user.openId)

						this.$Request.get("/app/UserVip/isUserVip").then(res => {
							if (res.code == 0 && res.data && res.data.isVip == 2) {
								uni.setStorageSync('isVIP', true)
							} else {
								uni.setStorageSync('isVIP', false)
							}
						});
						uni.navigateBack();
					} else {
						uni.showModal({
							showCancel: false,
							title: '登录失败',
							content: res.msg,
						});
					}
				})
			},

			weixinLo() {
				let that = this;
				uni.login({
					provider: 'weixin',
					success: function(loginRes) {
						that.$queue.showLoading('正在登录中...');
						console.error(loginRes.authResult);
						that.$queue.setData('weixinToken', loginRes.authResult.access_token);
						that.$queue.setData('unionid', loginRes.authResult.unionid);
						that.$queue.setData('weixinOpenid', loginRes.authResult.openid);
						that.$Request.postJson('/app/login/loginApp', {
							token: loginRes.authResult.access_token,
							unionid: loginRes.authResult.unionid,
							openid: loginRes.authResult.openid
						}).then(res => {
							console.log(JSON.stringify(res))
							if (res.code === 0) {
								if (uni.getSystemInfoSync().platform == "android") {
									let clientid = plus.push.getClientInfo().clientid;
									that.$Request.postT('/app/login/updateClientId?clientId=' +
										clientid + '&userId=' + res.userId).then(res => {

									});
								}
								that.$queue.setData("token", res.uuid);
								that.$queue.setData("userId", res.userId);
								that.getUserInfo(res.userId, res.token);
							} else {
								uni.hideLoading();
								uni.navigateTo({
									url: '/pages/public/wxmobile'
								});
							}
						});


					}
				});
			},

			forget() {
				uni.navigateTo({
					url: '/pages/public/pwd'
				});
			},
			register() {
				if (this.showAgree == false) {
					uni.showToast({
						icon: 'none',
						position: 'bottom',
						title: '请先同意《用户协议》和《隐私政策》'
					});
					return false;
				}
				uni.navigateTo({
					url: '/pages/public/loginphone'
				});
			},
			inputChange(e) {
				const key = e.currentTarget.dataset.key;
				this[key] = e.detail.value;
			},
			navBack() {
				uni.navigateBack();
			},

			getUserInfo(userId, token) {
				this.$Request.postJson('/app/selectUserById?userId=' + userId).then(res => {
					if (res.code === 0) {
						this.$queue.setData('token', res.data.uuid);
						this.$queue.setData('image_url', res.data.imageUrl ? res.data.imageUrl :
							'/static/img/common/logo.jpg');
						this.$queue.setData('inviterCode', res.data.inviterCode);
						this.$queue.setData('invitationCode', res.data.invitationCode);
						this.$queue.setData('grade', res.data.grade);
						this.$queue.setData('mobile', res.data.mobile);
						this.$queue.setData('isInvitation', res.data.isInvitation);
						this.$queue.setData('nickName', res.data.nickName ? res.data.nickName : res.data.phone);
						this.$queue.setData('gender', parseInt(res.data.gender));
						uni.switchTab({
							url: '/pages/index/index'
						});
					} else {
						uni.showModal({
							showCancel: false,
							title: '登录失败',
							content: res.msg
						});
						this.$queue.logout();
					}
					uni.hideLoading();
				});
			}
		}
	};
</script>

<style lang="scss">
	page {
		height: 100%;
		background: #FFFFFF !important;
	}

	.footer {
		// padding-left: 140upx;
		margin-top: 32upx;
		font-size: 24upx;
		color: #666666;
		// text-align: center;
		display: flex;
		align-items: center;
		justify-content: center;
	}


	.container {
		top: 0;
		padding-top: 50px;
		position: relative;
		width: 100%;
		height: 100%;
		overflow: hidden;
		background: #FFFFFF !important;
		color: #FFF;
	}



	.confirm-btn-weixin {
		width: 200px;
		height: 42px;
		line-height: 42px;
		border-radius: 30px;
		margin-top: 40upx;
		background: -moz-linear-gradient(left, #f15b6c, #e10a07 100%);
		background: -webkit-gradient(linear, left top, left right, color-stop(0, #f15b6c), color-stop(100%, #e10a07));
		background: -webkit-linear-gradient(left, #f15b6c 0, #e10a07 100%);
		background: -o-linear-gradient(left, #f15b6c 0, #e10a07 100%);
		background: -ms-linear-gradient(left, #f15b6c 0, #e10a07 100%);
		background: linear-gradient(to left, #f15b6c 0, #e10a07 100%);
		color: #fff;
		font-size: 32upx;

		&:after {
			border-radius: 60px;
		}
	}

	.confirm-btn {
		width: 200px;
		height: 42px;
		line-height: 42px;
		border-radius: 30px;
		margin-top: 300upx;
		background: -moz-linear-gradient(left, #f15b6c, #e10a07 100%);
		background: -webkit-gradient(linear, left top, left right, color-stop(0, #f15b6c), color-stop(100%, #e10a07));
		background: -webkit-linear-gradient(left, #f15b6c 0, #e10a07 100%);
		background: -o-linear-gradient(left, #f15b6c 0, #e10a07 100%);
		background: -ms-linear-gradient(left, #f15b6c 0, #e10a07 100%);
		background: linear-gradient(to left, #f15b6c 0, #e10a07 100%);
		color: #fff;
		font-size: 32upx;

		&:after {
			border-radius: 60px;
		}
	}

	.headers {
		text-align: center;
	}

	.headers>image {
		width: 400upx;
		height: 400upx;
	}

	// .footer {
	// 	padding-left: 100upx;
	// 	margin-top: 32upx;
	// 	font-size: 24upx;
	// 	color: #666666;
	// 	text-align: center;
	// 	display: flex;
	// }

	page {
		background: #fff;
	}

	.send-msg {
		border-radius: 30px;
		color: black;
		background: white;
		height: 30px;
		font-size: 14px;
		line-height: 30px;
	}

	.container {
		top: 0;
		padding-top: 32upx;
		position: relative;
		width: 100%;
		height: 100%;
		overflow: hidden;
		background: #ffffff;
		color: #FFF;

		.mp_wxBox {
			.headers {
				margin: 35% auto 50rpx;
				text-align: center;
				border-radius: 60rpx;
				width: 650rpx;
				height: 300rpx;
				line-height: 450rpx;

				image {
					width: 300rpx;
					height: 300rpx;
				}
			}

			.content {
				text-align: center;
			}

			text {
				display: block;
				color: #9d9d9d;
				margin-top: 40rpx;
			}

			.bottom {
				line-height: 80upx;
				border-radius: 80upx;
				margin: 70rpx 50rpx;
				height: 80upx;
				font-size: 35rpx;
			}
		}
	}

	.wrapper {
		position: relative;
		z-index: 90;
		background: #fff;
		padding-bottom: 20px;
	}

	.input-content {
		padding: 0 20px;
	}

	.confirm-btn {
		width: 300px;
		height: 42px;
		line-height: 42px;
		border-radius: 30px;
		margin-top: 40px;
		background: linear-gradient(to left, #3f5ecb 0, #5074FF 100%);
		color: #fff;
		// font-size: $font-lg;

		&:after {
			border-radius: 60px;
		}
	}
</style>