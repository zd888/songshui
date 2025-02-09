<template>
	<view style="padding-bottom: 150upx;">

		<view style="height: max-content;background-color: #FFFFFF;margin-top: 10upx;padding: 30upx 30upx 10upx 40upx;"
			v-for="(item,index) in list" :key='index'>
			<view class="flex align-center justify-between" @tap='goBackByAddress(item.addressId)'>
				<view style="width: 80%;">
					<view class="flex align-end text-lg text-bold" style="color: #333333;">
						<view>{{item.name}}</view>
						<view class="margin-left-sm">{{item.phone}}</view>
					</view>
					<view class="margin-top">
						{{item.province}}{{item.city}}{{item.district}} {{item.detailsAddress}}
					</view>
				</view>
				<view style="width: 1rpx;height: 66upx;background: #E6E6E6;"></view>
				<view @tap.stop='goAddress(item.addressId)'>
					<image src="../static/updete.png" style="width: 34upx;height: 34upx;"></image>
				</view>
			</view>
			<view style="margin-top: 15upx;height: 1upx;background-color: #E3E4E5;margin-bottom: 10upx;"></view>
			<view class="flex align-center justify-between">
				<radio-group name="openWay" style="text-align: left;width: 70%;">
					<label class="tui-radio" v-if="item.isDefault == '1'">
						<radio :checked="item.isDefault == 1 ? true : false" active-color="#557EFD" disabled='true'
							style="transform:scale(0.8);" />
						<text style="margin-left: 10upx;color: #999999;">默认地址</text>
					</label>
				</radio-group>
				<view v-if="isfa !=1" class="padding-tb-xs" style="color: #999999;">
					<view @tap='deleteAddressList(item.addressId)'>删除</view>
				</view>
			</view>
		</view>
		<view style="position: fixed;bottom: 0rpx;left: 0;right: 0;z-index: 999;">
			<button style="background: #025EFD;color: #FFFFFF;margin: 35rpx;width: 90%;border-radius: 44upx;"
				@tap.stop="goAddress('')">
				添加地址
			</button>
		</view>

		<empty v-if="list.length == 0"></empty>
		<!-- 悬浮上拉 -->
		<view class="scroll_top" @tap="topScrollTap" v-bind:class="[scrollTop ? 'active' : '', '']"
			style="bottom: 56px;"><text class="iconfont icon-shangla"></text></view>
	</view>
</template>

<script>
	import empty from '../../components/empty.vue'
	export default {
		components: {
			empty
		},
		data() {
			return {
				openWay: 0,
				list: [],
				// loadingType: 0,
				type: 1,
				scrollTop: false,
				page: 1,
				limit: 5,
				address: '',
				isfa: ''
			}
		},
		onShow() {
			let userId = this.$queue.getData('userId');
			if (userId) {
				this.getAddressList('');
			}
		},
		onLoad(e) {
			this.isfa = e.id

			console.log(this.isfa)
			let userId = this.$queue.getData('userId');
			if (userId) {
				this.getAddressList('');
			}
		},
		methods: {
			goBackByAddress(addressId) {
				console.log(addressId)
				this.$queue.setData('EditAddress', addressId);
				console.log(this.isfa)
				if (this.isfa == 0) {
					console.log('1111111')
				}
				if (this.isfa == 1) {
					uni.navigateBack()
				} else if (this.isfa == 3) {
					this.updateaddress()
					// uni.navigateBack()
				}

			},
			updateaddress() {
				let addressId = this.$queue.getData('EditAddress')
				let data = {
					ordersId: this.isfa.order,
					addressId: addressId
				}
				this.$Request.post('/app/orders/updateOrdersAddress', data).then(res => {
					console.log(res)
					if (res.code == 0) {
						uni.showToast({
							title: '修改成功',
							icon: 'none'
						})
						uni.navigateBack();


					}
				})

			},
			deleteAddressList(id) {
				console.log(id)
				var id = id
				let data = {
					addressId: id,
				}
				uni.showModal({
					title: '温馨提示',
					content: '您确定要删除此地址信息吗?',
					showCancel: true,
					cancelText: '取消',
					confirmText: '确认',
					success: res => {
						if (res.confirm) {
							this.$Request.postT('/app/address/deleteAddress', data).then(res => {
								console.log(res)
								if (res.code == 0) {
									this.$queue.showToast("删除成功！");
									this.getAddressList();
								} else {
									this.$queue.showToast(res.msg);
								}
							});
						}
					}
				});
			},
			getAddressList() {
				let userId = this.$queue.getData('userId');
				let data = {
					userId: userId,
					page: this.page,
					limit: this.limit
				}
				this.$Request.getT('/app/address/selectAddressListById', data).then(res => {
					console.log(res)
					uni.stopPullDownRefresh(); // 停止刷新
					if (res.code == 0) {
						if (this.page == 1) this.list = []
						this.list = [...this.list, ...res.data.list]; //追加新数据
					}
					uni.hideLoading();
					uni.stopPullDownRefresh(); // 停止刷新
				});
			},
			goAddress(id) {
				uni.navigateTo({
					url: './Endaddress?id=' + id
					// url: '/my/address/Endaddress?id=' + id
				});
			},
			tabSlect(item) {
				this.tabIndex = item.id;
			},
			selectWay(id) {
				this.openWay = id;
			},
			// topScrollTap: function() {
			// 	uni.pageScrollTo({
			// 		scrollTop: 0,
			// 		duration: 300
			// 	});
			// }
		},
		onReachBottom: function() {
			this.page = this.page + 1;
			this.getAddressList()
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.getAddressList()
		},
	}
</script>

<style lang="less">
	// @import '../../../static/less/index.less';
	// @import '../../../static/css/index.css';

	page {
		background: #F2F2F2;
	}

	.tui-line-cell {
		width: 100%;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-bottom: 2upx solid #f2f2f2;
		padding: 0 0 16upx 0;
	}

	.tui-title {
		line-height: 32rpx;
		min-width: 120rpx;
		flex-shrink: 0;
	}

	.tui-input {
		font-size: 32rpx;
		color: #333;
		padding-left: 20rpx;
		flex: 1;
	}
</style>
