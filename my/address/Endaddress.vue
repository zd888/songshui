<template>
	<view>
		<view class="part1 padding-lr">
			<view class="flex align-center padding-top">
				<view style="width: 120rpx;">收货人</view>
				<view>
					<input v-model="name" placeholder="请填写收货人姓名"></input>
				</view>
			</view>
			<view class="margin-top-xs" style="width: 100%;height: 1rpx;background: #f5f5ff;"></view>
			<view class="flex align-center padding-top">
				<view style="width: 120rpx;">手机号码</view>
				<view>
					<input v-model="mobile" placeholder="请填写收货人手机号" maxlength="11"></input>
				</view>
			</view>
			<view class="margin-top-xs" style="width: 100%;height: 1rpx;background: #f5f5ff;"></view>
			<view class="flex align-center padding-top" @click="bindmap">
				<view style="width: 120rpx;">选择地区</view>
				<view>
					<input v-model="cityaddress" placeholder="请填写收货地区" :disabled="true"></input>
				</view>
			</view>
			<view class="margin-top-xs" style="width: 100%;height: 1rpx;background: #f5f5ff;"></view>
			<view class="flex align-center padding-top">
				<view style="width: 120rpx;">详细地址</view>
				<view>
					<input v-model="detailaddress" placeholder="详细地址"></input>
				</view>
			</view>
			<view
				style="height: 100upx;background:#FFFFFF;display: flex;margin-top: 30upx;justify-content: space-between;padding: 0rpx 0rpx;">
				<view style="font-size: 34upx;color: #333333;text-align: left;width: 70%;padding: 20upx 0;">设为默认地址
				</view>
				<switch type="switch" :checked='isDefault === 1 ? true : false' @change="switch1Change"
					style="padding: 20upx;transform:scale(0.9)" color="#557EFD" />
			</view>
			<view class="btn" @click="bindhelp">确定</view>
		</view>

		<!-- <button
			style="width: 85%;height: 90upx;background: #FF332F;font-size: 32upx;color: #FFFFFF;margin-top: 40upx;border-radius: 50upx;line-height: 100upx;"
			@tap='setEditAddress'>保存</button> -->
	</view>
</template>

<script>
	// import pickerAddress from '@/components/wangding-pickerAddress/wangding-pickerAddress.vue'
	import pickerAddress from '@/my/components/wangding-pickerAddress/wangding-pickerAddress.vue'
	export default {
		components: {
			pickerAddress
		},
		data() {
			return {
				provinces: '',
				// consignee: '',
				// mobile: '',
				// detail: '',
				// createAt: '',
				id: '',
				isDefault: 0,
				// id: 0,
				mobile: '',
				name: '',
				cityaddress: '',
				detailaddress: '',
				latitude: '',
				longitude: '',
				province: '',
				city: '',
				district: ''
			}
		},
		onLoad(d) {
			let id = d.id;
			if (id) {
				this.id = d.id;
				uni.setNavigationBarTitle({
					title: '编辑地址'
				});
				this.getAddressList(id);
			} else {
				uni.setNavigationBarTitle({
					title: '添加地址'
				});
			}
		},
		methods: {
			change(data) {
				this.provinces = data.data.join('')
			},
			bindmap() {
				var that = this
				// if (that.ciaddress == '') {
				uni.chooseLocation({
					success: function(res) {
						// console.log(res)
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
						that.detailaddress = res.name
						that.latitude = res.latitude
						that.longitude = res.longitude
						that.shengcheng(res.longitude, res.latitude)
					}
				});
				// }
			},
			shengcheng(longitude, latitude) {
				this.$Request.getT('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					// console.log(res)
					if (res.code === 0) {
						this.cityaddress = res.data.province + res.data.city + res.data.district
						// console.log(this.address)
						this.province = res.data.province
						this.city = res.data.city
						this.district = res.data.district
					}
				});

			},
			getAddressList(id) {
				uni.showLoading({
					title: '加载中...'
				});
				this.$Request.getT('/app/address/selectAddressByAddressId?addressId=' + id).then(res => {
					console.log(res)
					if (res.code == 0) {
						this.name = res.data.name;
						this.mobile = res.data.phone;
						this.cityaddress = res.data.province + res.data.city + res.data.district;
						this.detailaddress = res.data.detailsAddress;
						this.isDefault = res.data.isDefault;
						this.userId = res.data.userId;
						this.latitude = res.data.latitude;
						this.longitude = res.data.longitude;
						this.province = res.data.province
						this.city = res.data.city
						this.district = res.data.district
						this.provinces = res.data.province + res.data.city + res.data.district;
					}
					uni.hideLoading();
				});
			},
			bindhelp() {
				if (this.id) {
					this.$queue.showLoading('修改中...')
				} else {
					this.$queue.showLoading('添加中...')
				}
				let userId = this.$queue.getData('userId');
				let data = {
					name: this.name,
					phone: this.mobile,
					province: this.province,
					city: this.city,
					district: this.district,
					detailsAddress: this.detailaddress,
					isDefault: this.isDefault,
					addressId: this.id,
					// address: this.cityaddress,
					// addressDetail: this.detailaddress,
					latitude: this.latitude,
					longitude: this.longitude,
					// userId: userId,

					// addressDefault: this.isDefault
				}
				if (this.mobile.length < 11) {
					uni.showToast({
						icon: 'none',
						title: '请输入正确的电话信息'
					});
					return;
				}
				if (this.name != undefined && this.name != '') {} else {
					uni.showToast({
						icon: 'none',
						title: '收货人不能为空'
					});
					return;
				}
				if (this.cityaddress == '选择地址') {
					uni.showToast({
						icon: 'none',
						title: '请选择地址信息'
					});
					return;
				}
				if (this.detailaddress != undefined && this.detailaddress != '') {} else {
					uni.showToast({
						icon: 'none',
						title: '地址信息不能为空'
					});
					return;
				}

				if (this.id) {
					this.$Request.postJson('/app/address/updateAddress', data).then(res => {
						if (res.code == 0) {
							uni.hideLoading()
							this.$queue.showToast("修改成功!");
							setTimeout(d => {
								uni.navigateBack();
							}, 1000)
						} else {
							uni.hideLoading()
							this.$queue.showToast(res.msg);
						}
					});
				} else {
					this.$Request.postJson('/app/address/insertAddress', data).then(res => {
						if (res.code == 0) {
							uni.hideLoading()
							this.$queue.showToast("添加成功!");
							setTimeout(d => {
								uni.navigateBack();
							}, 1000)
						} else {
							uni.hideLoading()
							this.$queue.showToast(res.msg);
						}
					});
				}
			},
			//校验手机格式
			checkMobile(mobile) {
				return RegExp(/^1[34578]\d{9}$/).test(mobile);
			},
			switch1Change(e) {
				this.isDefault = e.detail.value == true ? 1 : 0;
			}
		}
	}
</script>

<style>
	page {
		background: #F5F5F5;
	}

	.part1 {
		width: 100%;
		background: #ffffff;
		margin-top: 24rpx;
		padding-bottom: 40upx;

	}

	.btn {
		width: 90%;
		height: 80upx;
		background: #025EFD;
		border-radius: 14upx;
		margin: 0 auto;
		color: white;
		text-align: center;
		line-height: 80upx;
		letter-spacing: 2upx;
		border-radius: 44upx;
		position: fixed;
		bottom: 60upx;
		left: 0;
		right: 0;
		z-index: 99;
	}

	.u-icon__icon {
		font-size: 45rpx !important;
	}

	.u-field {
		padding: 35rpx 0rpx !important;
	}

	.u-label {
		flex: 0 0 42px !important;
	}

	.u-field__input-wrap {
		font-size: 30rpx !important;
	}

	.u-textarea-class {
		font-size: 30rpx !important;
	}

	.u-label-text span {
		font-size: 32upx !important;
		font-weight: bold !important;
	}
</style>
