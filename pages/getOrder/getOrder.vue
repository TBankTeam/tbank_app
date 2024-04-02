<template>
	<view style="padding: 20rpx;margin-top: 180rpx;">
		<!-- 顶部自定义导航 -->
		<tn-nav-bar fixed alpha customBack>
			<view slot="back" class='tn-custom-nav-bar__back' @click="goBack()">
				<text class='icon tn-icon-left'></text>
				<text class='icon tn-icon-home-capsule-fill'></text>
			</view>
		</tn-nav-bar>
		<view class="box" style="color: #006eff; font-weight: bold; margin-bottom: 10rpx;">服务订单</view>
		<view>
			<view v-for="item in orderList" :key="item.id" class="box" style="margin-bottom: 10rpx;"
				@click="goDetail(item.id)">
				<view style="display: flex; align-items: center; margin-bottom: 20rpx;">
					<view style="flex: 1;">
						<uni-tag text="餐品" size="small" type="success" v-if="item.type === '代取餐品'"></uni-tag>
						<uni-tag text="快递" size="small" type="primary" v-if="item.type === '代拿快递'"></uni-tag>
						<uni-tag text="零食" size="small" type="warning" v-if="item.type === '代买零食'"></uni-tag>
						<uni-tag text="鲜花" size="small" type="error" v-if="item.type === '代送鲜花'"></uni-tag>
						<text style="margin-left: 10rpx;">{{ item.name }}</text>
					</view>
					<view style="flex: 1; text-align: right;">
						<text style="color: #888;">时间币</text>
						<text style="color: red; font-size: 34rpx;">￥{{ item.price }}</text>
					</view>
				</view>

				<view style="display: flex; align-items: center;">
					<view style="flex: 1;">
						<text style="margin-right: 10rpx;">已下单{{ item.range }}分钟</text>
						<text style="color: orange;">待接单</text>
					</view>
					<view style="flex: 1; text-align: right;">
						<uni-tag text="接单" type="primary" size="small" @click.native.stop="accept(item)"></uni-tag>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				orderList: [],
				user: uni.getStorageSync('xm-user'),
			}
		},
		onShow() {
			this.load()
		},
		onHide() {
			clearInterval(this.inter)
			this.inter = null
		},
		methods: {
			accept(orders) {
				if (!this.user.isRider) { // 判断是否是骑手
					uni.showToast({
						icon: 'none',
						title: '只有实名认证才可以接单'
					})
					return
				}
				this.$request.put('/orders/accept', orders).then(res => {
					if (res.code === '200') {
						uni.showToast({
							icon: 'success',
							title: '操作成功'
						})
						this.load()
					} else {
						uni.showToast({
							icon: 'none',
							title: res.msg
						})
					}
				})
			},
			goDetail(orderId) {
				uni.navigateTo({
					url: '/pages/detail/detail?orderId=' + orderId
				})
			},
			load() {
				this.$request.get('/orders/selectAll', {
					status: '待接单'
				}).then(res => {
					this.orderList = res.data || []
				})
			},
			goBack() {
				setTimeout(() => {
					uni.switchTab({
						url: '/pages/indexplus/indexplus'
					})
				}, 500)
			}

		}
	}
</script>

<style lang="scss" scoped>
	/* 胶囊*/
	.tn-custom-nav-bar__back {
		width: 100%;
		height: 100%;
		position: relative;
		display: flex;
		justify-content: space-evenly;
		align-items: center;
		box-sizing: border-box;
		background-color: rgba(0, 0, 0, 0.15);
		border-radius: 1000rpx;
		border: 1rpx solid rgba(255, 255, 255, 0.5);
		color: #FFFFFF;
		font-size: 18px;

		.icon {
			display: block;
			flex: 1;
			margin: auto;
			text-align: center;
		}

		&:before {
			content: " ";
			width: 1rpx;
			height: 110%;
			position: absolute;
			top: 22.5%;
			left: 0;
			right: 0;
			margin: auto;
			transform: scale(0.5);
			transform-origin: 0 0;
			pointer-events: none;
			box-sizing: border-box;
			opacity: 0.7;
			background-color: #FFFFFF;
		}
	}


	/* 底部悬浮按钮 start*/
	.tn-tabbar-height {
		min-height: 100rpx;
		height: calc(120rpx + env(safe-area-inset-bottom) / 2);
	}

	.tn-footerfixed {
		position: fixed;
		width: 100%;
		bottom: calc(60rpx + env(safe-area-inset-bottom));
		z-index: 1024;
		box-shadow: 0 1rpx 6rpx rgba(0, 0, 0, 0);

	}

	/* 底部悬浮按钮 end*/

	/* 卡 */
	.button-vip {
		width: 100%;
		height: 300rpx;
		border-radius: 15rpx;
		position: relative;
		z-index: 1;

		&::after {
			content: " ";
			position: absolute;
			z-index: -1;
			width: 100%;
			height: 100%;
			left: 0;
			bottom: 0;
			border-radius: inherit;
			opacity: 1;
			transform: scale(1, 1);
			background-size: 100% 100%;
			background-image: url(https://resource.tuniaokj.com/images/cool_bg_image/icon_bg4.png);
		}
	}


	/* 间隔线 start*/
	.tn-strip-bottom-min {
		width: 100%;
		border-bottom: 1rpx solid #F8F9FB;
	}

	.tn-strip-bottom {
		width: 100%;
		border-bottom: 20rpx solid rgba(241, 241, 241, 0.8);
	}

	/* 间隔线 end*/
</style>