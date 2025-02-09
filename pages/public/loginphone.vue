<template>
	<view class="container">
		<view class="wrapper">
			<!-- <view style="text-align: center;">
				<image src="../../static/logo.png" style="border-radius: 64upx;"></image>
			</view>
 -->
			<view class="input-content">
				<view class="cu-form-group"
					style="border: 2upx solid whitesmoke;margin-bottom: 20px;border-radius: 30px">
					<view class="title text-black">账号</view>
					<input type="number" :value="phone" placeholder="请输入手机号" maxlength="11" data-key="phone"
						@input="inputChange" />
				</view>
				<view class="cu-form-group" style="border: 2upx solid whitesmoke;border-radius: 30px">
					<view class="title text-black">密码</view>
					<input type="password" placeholder="请输入密码" maxlength="20" :value="password" data-key="password"
						@input="inputChange" @confirm="toLogin" />
					<text class="send-msg" @click="forget">忘记密码</text>
				</view>
			</view>
			<button class="confirm-btn" @click="toLogin">登录</button>

			<view style="margin-top: 32px;text-align: center">
				<view><text style="font-size: 28upx;">没有账号？</text>
					<text style="color: #557EFD" @click="register()">立即注册</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phone: '',
				password: '',
				banners: [],
				invitation: '',
				loginName: '',
				sending: false,
				sendTime: '获取验证码',
				count: 60,
			}
		},
		onLoad() {

		},
		methods: {

			forget() {
				uni.navigateTo({
					url: '/pages/public/forgetPwd'
				});
			},
			register() {
				uni.navigateTo({
					url: '/pages/public/register'
				});
			},
			inputChange(e) {
				const key = e.currentTarget.dataset.key;
				this[key] = e.detail.value;
			},
			navBack() {
				uni.navigateBack();
			},

			toLogin() {
				this.$queue.loginClear();
				let openid = this.$queue.getData("openid");
				const {
					phone,
					password
				} = this;
				if (!phone) {
					this.$queue.showToast("请输入手机号");
				} else if (phone.length != 11) {
					this.$queue.showToast("请输入正确的手机号");
				} else if (!password) {
					this.$queue.showToast("请输入密码");
				} else {
					this.$queue.showLoading("正在登录中...");
					this.$Request.post("/app/Login/loginApp", {
						password: password,
						phone: phone,
						openId: this.$queue.getData('openid')
					}).then(res => {
						if (res.code == 0) {
							this.$queue.setData("userId", res.user.userId);
							this.$queue.setData("token", res.token);
							this.$queue.setData("phone", res.user.phone);
							this.$queue.setData("userName", res.user.userName);
							this.$queue.setData("avatar", res.user.avatar);
							this.$queue.setData("invitationCode", res.user.invitationCode);
							this.$queue.setData("inviterCode", res.user.inviterCode);

							this.getIsVip()
							uni.hideLoading();
							uni.switchTab({
								url: '/pages/my/index'
							})
						} else {
							uni.hideLoading();
							this.$queue.showToast(res.msg);
						}
					});
				}
			},
			getIsVip() {
				this.$Request.get("/app/UserVip/isUserVip").then(res => {
					if (res.code == 0) {
						// this.isVip = res.data
						console.log(res.data)
						this.$queue.setData("isVip", res.data);
					}
				});
			}
		},

	}
</script>

<style lang='scss'>
	page {
		height: 100%;
		background: #F5F5F5 !important;
	}

	.bg {
		background-color: #FFFFFF;
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
		background: #FFFFFF !important;
	}

	.wrapper {
		position: relative;
		z-index: 90;
		background: #FFFFFF;
		padding-bottom: 32upx;
	}


	.input-content {
		/* margin-top: 300upx; */
		/* padding-top: 300upx; */
		padding: 32upx 80upx;

	}


	.confirm-btn {
		width: 600upx;
		height: 80upx;
		line-height: 80upx;
		border-radius: 60upx;
		margin-top: 32upx;
		background: #557EFD;
		color: #fff;
		font-size: 32upx;

		&:after {
			border-radius: 60px;
		}
	}
</style>
