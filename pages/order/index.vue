<!-- 菜单悬浮的原理: 通过给菜单添加position:sticky实现, 用法超简单, 仅APP端的低端机不兼容 https://caniuse.com/#feat=css-sticky -->
<template>
	<view style="padding-bottom:150upx;">
		<view class="listbox flex align-center" v-for="(item,index) in list" :key="index">
			<view style="width: 9%;">
				<u-checkbox-group @change="checkboxGroupChange">
					<u-checkbox @change="checkboxChange(item,index)" v-model="item.checked">
					</u-checkbox>
				</u-checkbox-group>
			</view>
			<view class="flex" style="width: 90%;">
				<image :src="item.homepageImg?item.homepageImg:'../../static/logo.png'"
					style="width: 160upx;height: 160upx;" mode="aspectFill"></image>
				<view class="margin-left-sm" style="width: 68%;">
					<view>{{item.serviceName}}</view>
					<view class="text-sm  align-center margin-top-xs" v-if="item.detailJson"
						style="color: #999999;border: 1rpx solid #808080;display: inline-flex;padding: 3upx 10upx;">
						<view class="margin-right-xs">{{item.detailJson}}</view>
						<!-- <image src="../../my/static/dowm.png" style="width: 14upx;height: 8upx;"></image> -->
					</view>
					<view class="flex align-end justify-between margin-top-sm">
						<view style="color: #FD3C44;font-size: 38upx;">￥{{item.money}}</view>
						<view class="flex align-center ">
							<view class="jia" @click.stop="minus(item,index)">-</view>
							<view class="content">{{item.orderNumber}}</view>
							<view class="jian" @click.stop="plus(item,index)">+</view>
						</view>
					</view>
					<!-- <view class="margin-top-xs" > 单价：<text style="color: #FD3C44">￥{{item.money}}</text></view> -->
					<!-- <view class="text-right">
						<view style="color: #FD3C44;font-size: 38upx;">￥{{item.payMoney}}</view>
					</view> -->
				</view>
			</view>
		</view>
		<empty v-if="list.length == 0"></empty>
		<view class="tanner" v-if="list.length">
			<view class="shuibe">
				<view class="flex align-center">
					<image src="../../static/images/my/shuibei.png" style="width: 34upx;height: 38upx;"></image>
					<view class="text-sm margin-left-xs" style="color: #000000;">可用水贝支付</view>
				</view>
				<view>当前余额：<text style="color: #025EFD;">{{money?money:'0'}}</text></view>
			</view>

			<view class="bg flex ">
				<view class="flex align-center justify-between padding-left margin-right" style="width: 70%;">
					<view @click="checkedAll()">
						<u-checkbox-group>
							<u-checkbox v-model="Allchecked" name='1'>全选</u-checkbox>
						</u-checkbox-group>
					</view>
					<view style="color: #C6142A;font-size: 38upx;font-weight: bold;">￥{{totalMoney}}</view>
				</view>
				<view class="shopcart" style="width: 30%;" @click="goOrderDet()">去结算</view>
			</view>
		</view>

	</view>
</template>

<script>
	import empty from '../../components/empty.vue'
	export default {
		components: {
			empty,
		},
		data() {
			return {
				page: 1,
				limit: 10,
				count: '',
				list: [],
				orderList: [],

				number: '1',
				Allchecked: false,
				money: 0,
				totalMoney: 0,
				userId: ''
			}
		},
		onLoad() {



		},
		onShow() {
			this.userId = uni.getStorageSync('userId')
			if (this.userId) {
				this.page = 1;
				this.shopList();
				this.getMyMoney()
				this.Allchecked = false
				this.totalMoney = 0
			}
		},
		methods: {
			
			//购物车下单
			goOrderDet() {
				// this.showpay = true
				let dataList = [];
				this.orderList.forEach(d => {
					if (d.checked) {
						dataList.push(d);
					}
				});

				if (dataList.length == 0) {
					uni.showToast({
						title: '请选择需要下单的服务',
						icon: 'none'
					})
				} else {
					this.$queue.setData('orderList', dataList);
					uni.navigateTo({
						url: '/pages/order/orderDet?totalMoney=' + this.totalMoney
					})
					this.totalMoney = 0
					this.Allchecked = false
				}

			},
			//数量减少
			minus(e, index) {
				let that = this;
				console.log(index + '数量减少当前值为: ' + e.orderNumber + '------------' + e)
				if (parseInt(e.orderNumber) == 1 || parseInt(e.orderNumber) == 0) {
					console.error('sssssss')
					uni.showModal({
						title: '提示',
						content: '确定删除吗？',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								let data = {
									id: e.ordersId
								}
								that.$Request.get("/app/orders/deleteOrder", data).then(ret => {
									if (ret.code == 0) {
										// let list = that.list
										// for (var i = 0; i < list.length; i++) {
										// 	if (list[i].ordersId == e.ordersId) {
										// 		list[i].splice(i, 1)
										// 		// that.orderList[i].splice(i, 1)
										// 	}
										// }
										// that.list = list
										if (that.orderList[index].checked == true) {
											that.totalMoney = (parseFloat(that.totalMoney) -
												parseFloat(e.money)).toFixed(2)
											// that.totalMoney = (parseFloat(that.totalMoney) -
											// 		parseFloat(e.money)).toFixed(2)
										}
										that.shopList()
										uni.showToast({
											title: '删除成功！',
											icon: 'none'
										})
										console.error(that.list)

									}
								})

							} else if (res.cancel) {
								that.list[index].orderNumber = 1;
								that.orderList[index].orderNumber = 1;
								console.log('用户点击取消', );
							}
						}
					})
				} else {
					this.list[index].orderNumber = this.list[index].orderNumber * 1 - 1
					this.orderList[index].orderNumber = this.list[index].orderNumber
					let data = {
						id: e.ordersId,
						type: 2
					}
					that.$Request.post("/app/orders/updateShoppingNum", data).then(res => {
						if (res.code == 0) {
							if (that.orderList[index].checked == true) {
								that.orderList[index].payMoney = parseFloat(res.data).toFixed(2);

								that.totalMoney = (parseFloat(that.totalMoney) - parseFloat(that
									.orderList[index].money)).toFixed(2)
							} else {
								that.orderList[index].payMoney = parseFloat(res.data).toFixed(2);
							}
						} else {
							uni.hideLoading();
							// that.$queue.showToast(res.msg)
						}
					})
				}

			},
			//数量增加
			plus(e, index) {
				// console.log(index + '数量增加当前值为: ' + e.orderNumber + '---------' + e)
				this.list[index].orderNumber = this.list[index].orderNumber * 1 + 1
				this.orderList[index].orderNumber = this.list[index].orderNumber
				let data = {
					id: e.ordersId,
					type: 1
				}
				this.$Request.post("/app/orders/updateShoppingNum", data).then(res => {
					if (res.code == 0) {
						if (this.orderList[index].checked == true) {
							this.orderList[index].payMoney = parseFloat(res.data).toFixed(2);
							// let totalMoney = this.totalMoney
							this.totalMoney = (parseFloat(this.totalMoney) + parseFloat(this.orderList[index]
								.money)).toFixed(2)
							console.log(this.totalMoney, '88888888', this.totalMoney, this.orderList[index].money)
						} else {
							this.orderList[index].payMoney = parseFloat(res.data).toFixed(2);

						}
					} else {
						uni.hideLoading();
						this.$queue.showToast(res.msg)
					}
				});

			},
			// 选中某个复选框时，由checkbox时触发
			checkboxChange(e, selectIndex) {
				console.log('222222', e)
				let orderNumber = e.orderNumber
				if (e.checked != true) {
					this.orderList[selectIndex].checked = true;
					this.totalMoney = (parseFloat(this.totalMoney) + parseFloat(this.orderList[selectIndex].payMoney))
						.toFixed(2)
					console.log('33333333333', this.totalMoney)
				} else {
					this.orderList[selectIndex].checked = false;
					this.totalMoney = (parseFloat(this.totalMoney) - parseFloat(this.orderList[selectIndex].payMoney))
						.toFixed(2)

				}

			},
			// 选中任一checkbox时，由checkbox-group触发
			checkboxGroupChange(e) {
				console.log(e);
			},
			// 全选
			checkedAll() {
				console.log(this.list);
				// let sumMoney = 0;
				// this.orderList[selectIndex].checked = true;
				// this.totalMoney = (parseFloat(this.totalMoney) + parseFloat(this.orderList[selectIndex].payMoney)).toFixed(2)

				if (this.Allchecked == true) {
					this.totalMoney = 0;
					this.orderList.forEach(d => {
						d.checked = true;
						this.totalMoney = (parseFloat(this.totalMoney) + parseFloat(d.payMoney)).toFixed(2)
					});

				} else if (this.Allchecked == false) {
					this.totalMoney = 0
					this.orderList.forEach(d => {
						d.checked = false;
					});
				}
			},
			//购物车列表
			shopList() {
				let data = {
					page: this.page,
					limit: this.limit,
					status: 0,
					isShopping: 1
				}
				this.$Request.get("/app/orders/selectMyOrder", data).then(res => {
					if (res.code == 0) {
						if (this.page == 1) {
							this.list = []
						}
						res.data.list.forEach(d => {
							d.checked = false
							this.list.push(d)
							this.orderList = this.list;
						})
						this.count = res.data.totalCount
					} else {
						uni.hideLoading();
						this.$queue.showToast(res.msg)
					}
					uni.stopPullDownRefresh();
					uni.hideLoading();
				});
			},
			getMyMoney() {
				this.$Request.get("/app/userMoney/selectMyMoney").then(res => {
					if (res.code == 0&&res.data) {
						this.money = res.data.money
					}
				})
			},
		},
		onReachBottom: function() {
			if (this.count == this.list.length) {
				uni.showToast({
					title: '已经到底了',
					icon: 'none'
				})
			} else {
				this.page = this.page + 1;
				this.shopList()
			}
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.shopList()
		},
	}
</script>

<style lang="scss">
	page {
		background: #FAFAFA;
	}

	.bg {
		background: #ffffff;
	}

	.listbox {
		background: #ffffff;
		border-radius: 18upx;
		margin: 30upx;
		padding: 30upx 20upx;
	}

	.jian {
		width: 50upx;
		height: 48upx;
		border: 1upx solid #000000;
		text-align: center;
		line-height: 42upx;
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

	.tanner {
		position: fixed;
		/* #ifdef H5 */
		bottom: 50px;
		/* #endif */
		/* #ifndef H5 */
		bottom: 0px;
		/* #endif */
		left: 0;
		right: 0;
		z-index: 99;
	}

	.shuibe {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 10upx 30upx;
		background: #EAF7FA;
	}

	.shopcart {
		color: #ffffff;
		background: #025EFD;
		padding: 30upx 0upx;
		text-align: center;
	}
</style>
