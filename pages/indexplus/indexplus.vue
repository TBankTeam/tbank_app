<template>
	<view class="template-screen tn-safe-area-inset-bottom">
		<!-- 顶部自定义导航 -->
		<!-- <tn-nav-bar fixed alpha customBack>
      <view slot="back" class='tn-custom-nav-bar__back'
        @click="goBack">
        <text class='icon tn-icon-left'></text>
        <text class='icon tn-icon-home-capsule-fill'></text>
      </view>
    </tn-nav-bar> -->

		<!-- 顶部自定义导航 -->
		<tn-nav-bar fixed :isBack="false" :bottomShadow="false" backgroundColor="#3D7EFF80">
			<view class="custom-nav tn-flex tn-flex-col-center tn-flex-row-left">
				<!-- 按钮 -->
				<view class="custom-nav__back">
					<view class="tn-icon-caring tn-color-white" style="font-size: 60rpx;"></view>
				</view>
				<!-- 搜索框 -->
				<view class="custom-nav__search tn-flex tn-flex-col-center tn-flex-row-center ">
					<view class="custom-nav__search__box tn-flex tn-flex-col-center tn-flex-row-left tn-color-white">
						<view class="custom-nav__search__icon tn-icon-search"></view>
						<view class="custom-nav__search__text tn-padding-left-xs">试试搜索“志愿活动”</view>
					</view>
				</view>
			</view>
		</tn-nav-bar>


		<!-- 页面内容 -->
		<view class="bg-contaniner tn-bg-blue">

		</view>
		<view class="tn-margin-top-lg" style="padding-bottom: 10rpx;">
			<view class="tn-padding" :style="{paddingTop: vuex_custom_bar_height + 'px'}">
				<tn-swiper :list="banner" :height="350" :effect3d="false" mode="dot"></tn-swiper>
			</view>

		</view>
		<view style="margin-bottom: 0rpx;">
			<uni-notice-bar v-if="content" show-icon single :text="content" />
		</view>
		<!-- <view class="tn-bg-white" style="margin-top: -100rpx;border-radius: 50rpx 50rpx 0 0;height: 600px;"> -->
		<view class="tn-bg-white" style="margin-top: -100rpx;height: 500px;">
			<!-- 方式7 start-->
			<view class="tn-flex tn-margin-xs tn-padding-top-xl">
				<view class="tn-flex-1 screen-shadow" style="margin: 30rpx 20rpx;padding: 60rpx 0;">
					<view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center"
						@click="goPreOrder()">
						<view class="icon7__item--icon tn-flex tn-flex-row-center tn-flex-col-center">
							<image class="" src='https://resource.tuniaokj.com/images/icon/5.jpg' mode='aspectFit'>
							</image>
						</view>
						<view class="tn-text-center">
							<view class="tn-text-ellipsis tn-color-black tn-text-lg ">我要发单</view>
							<view class="tn-text-ellipsis tn-color-gray--dark">老人发布需求</view>
						</view>
					</view>
				</view>
				<view class="tn-flex-1 screen-shadow" style="margin: 30rpx 20rpx;padding: 60rpx 0;">
					<view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center"
						@click="goGetOrder()">
						<view class="icon7__item--icon tn-flex tn-flex-row-center tn-flex-col-center">
							<image class="" src='https://resource.tuniaokj.com/images/icon/6.jpg' mode='aspectFit'>
							</image>
						</view>
						<view class="tn-text-center">
							<view class="tn-text-ellipsis tn-color-black tn-text-lg ">我要接单</view>
							<view class="tn-text-ellipsis tn-color-gray--dark">志愿者服务</view>
						</view>
					</view>
				</view>
			</view>

			<view class="tn-flex tn-margin-xs" style="margin-top: -10rpx;">
				<view class="tn-flex-1  screen-shadow" style="margin: 10rpx 20rpx;padding: 60rpx 0;">
					<view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center"
						@click="goActivity()">
						<view class="icon7__item--icon tn-flex tn-flex-row-center tn-flex-col-center">
							<image class="" src='https://resource.tuniaokj.com/images/icon/7.jpg' mode='aspectFit'>
							</image>
						</view>
						<view class="tn-text-center">
							<view class="tn-text-ellipsis tn-color-black tn-text-lg ">活动报名</view>
							<view class="tn-text-ellipsis tn-color-gray--dark">活动报名</view>
						</view>
					</view>
				</view>
				<view class="tn-flex-1  screen-shadow" style="margin: 10rpx 20rpx;padding: 60rpx 0;">
					<view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center"
						@click="goNews()">
						<view class="icon7__item--icon tn-flex tn-flex-row-center tn-flex-col-center">
							<image class="" src='https://resource.tuniaokj.com/images/icon/8.jpg' mode='aspectFit'>
							</image>
						</view>
						<view class="tn-text-center">
							<view class="tn-text-ellipsis tn-color-black tn-text-lg ">查看资讯</view>
							<view class="tn-text-ellipsis tn-color-gray--dark">查看资讯</view>
						</view>
					</view>
				</view>
			</view>
			<!-- 方式7 end-->

			<!-- <view class="box" style="color: #006eff; font-weight: bold; margin-bottom: 10rpx;">服务订单</view>
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
			</view> -->



		</view>



		<!-- 回到首页悬浮按钮-->
		<nav-index-button></nav-index-button>

	</view>
</template>

<script>
	import template_page_mixin from '@/libs/mixin/template_page_mixin.js'
	import NavIndexButton from '@/libs/components/nav-index-button.vue'
	export default {
		name: 'TemplateScreen',
		mixins: [template_page_mixin],
		components: {
			NavIndexButton
		},
		data() {
			return {
				content: '',
				noticeList: [],
				inter: null,
				orderList: [],
				user: uni.getStorageSync('xm-user'),
				banner: [{
					image: 'https://resource.tuniaokj.com/images/shop/banner1.jpg'
				}, {
					image: 'https://resource.tuniaokj.com/images/shop/banner2.jpg'
				}, {
					image: 'https://resource.tuniaokj.com/images/shop/banner1.jpg'
				}, {
					image: 'https://resource.tuniaokj.com/images/shop/banner2.jpg'
				}],

			}
		},
		onShow() {
			this.load()
			this.loadNotice()
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
						title: '只有认证骑手才可以接单'
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
			goPreOrder(type) {
				let orderStore = uni.getStorageSync('orderStore') || {} // 先获取缓存的数据
				orderStore.type = type // 设置订单的类型
				uni.setStorageSync('orderStore', orderStore)
				uni.navigateTo({
					url: '/pages/preOrder/preOrder'
				})
			},
			goGetOrder() {
				uni.navigateTo({
					url: '/pages/getOrder/getOrder'
				})
			},
			goActivity() {
				uni.navigateTo({
					url: '/pages/activity/activity'
				})
			},
			goNews() {
				uni.navigateTo({
					url: '/pages/news/news'
				})
			},
			load() {
				this.$request.get('/orders/selectAll', {
					status: '待接单'
				}).then(res => {
					this.orderList = res.data || []
				})
			},
			loadNotice() {
				this.$request.get('/notice/selectAll').then(res => {
					this.noticeList = res.data || []

					let i = 0
					this.content = this.noticeList.length ? this.noticeList[i].content : ''

					// 切换展示公告内容
					if (this.noticeList.length > 1) {
						this.inter = setInterval(() => {
							i++
							if (i === this.noticeList.length) {
								i = 0
							}
							this.content = this.noticeList[i].content
						}, 5000)
					}

				})
			}

		}
	}
</script>

<style lang="scss" scoped>
	@import '@/static/css/templatePage/custom_nav_bar.scss';

	/* 自定义导航栏内容 start */
	.custom-nav {
		height: 100%;

		&__back {
			margin: auto 5rpx;
			font-size: 40rpx;
			margin-right: 10rpx;
			margin-left: 30rpx;
			flex-basis: 5%;
		}

		&__search {
			flex-basis: 60%;
			width: 100%;
			height: 100%;

			&__box {
				width: 100%;
				height: 70%;
				padding: 10rpx 0;
				margin: 0 30rpx;
				border-radius: 60rpx 60rpx 60rpx 0;
				font-size: 24rpx;
				background-color: rgba(255, 255, 255, 0.1);
			}

			&__icon {
				padding-right: 10rpx;
				margin-left: 20rpx;
				font-size: 30rpx;
			}

			&__text {}
		}
	}

	/* 自定义导航栏内容 end */

	.screen-shadow {
		box-shadow: 0rpx 0rpx 80rpx 0rpx rgba(0, 0, 0, 0.07);
		border-radius: 20rpx;
	}

	/* 图标容器15 start */
	.icon15 {
		&__item {
			width: 30%;
			background-color: #FFFFFF;
			border-radius: 10rpx;
			padding: 30rpx;
			margin: 20rpx 10rpx;
			transform: scale(1);
			transition: transform 0.3s linear;
			transform-origin: center center;

			&--icon {
				width: 100rpx;
				height: 100rpx;
				font-size: 60rpx;
				border-radius: 50%;
				margin-bottom: 18rpx;
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


				}
			}
		}
	}

	/* 图标容器7 start */
	.icon7 {
		&__item {
			width: 30%;
			background-color: #FFFFFF;
			border-radius: 10rpx;
			padding: 10rpx;
			margin: 20rpx 10rpx;
			transform: scale(1);
			transition: transform 0.3s linear;
			transform-origin: center center;

			&--icon {
				width: 100rpx;
				height: 100rpx;
				font-size: 60rpx;
				border-radius: 0;
				margin-bottom: 18rpx;
				position: relative;
				z-index: 1;
			}
		}
	}

	/* 文章内容 start*/
	.tn-blogger-content {
		&__wrap {
			box-shadow: 0rpx 0rpx 80rpx 0rpx rgba(0, 0, 0, 0.09);
			border-radius: 20rpx;
			margin: 30rpx;
		}

		&__info {
			&__btn {
				margin-right: -12rpx;
				opacity: 0.5;
			}
		}

		&__label {
			&__item {
				line-height: 45rpx;
				padding: 0 20rpx;
				margin: 5rpx 18rpx 0 0;

				&--prefix {
					color: #00FFC8;
					padding-right: 10rpx;
				}
			}

			&__desc {
				line-height: 55rpx;
			}
		}

		&__main-image {
			border-radius: 16rpx 16rpx 0 0;

			&--1 {
				max-width: 690rpx;
				min-width: 690rpx;
				max-height: 400rpx;
				min-height: 400rpx;
			}

			&--2 {
				max-width: 260rpx;
				max-height: 260rpx;
			}

			&--3 {
				height: 212rpx;
				width: 100%;
			}
		}

		&__count-icon {
			font-size: 40rpx;
			padding-right: 5rpx;
		}
	}

	.image-screen {
		padding: 180rpx 0rpx;
		font-size: 40rpx;
		font-weight: 300;
		position: relative;
	}

	.image-pic {
		background-size: cover;
		background-repeat: no-repeat;
		// background-attachment:fixed;
		background-position: top;
		border-radius: 10rpx;
	}

	/* 文章内容 end*/

	/* 移动背景部分 start*/
	.bg-contaniner {
		position: fixed;
		top: -0rpx;
		left: -300rpx;
		--text-color: hsl(0 95% 60%);
		--bg-color: hsl(0 0% 100%);
		--bg-size: 200px;
		height: 100%;
		display: grid;
		place-items: center;
		place-content: center;
		overflow: hidden;
		background-color: var(--bg-color);
		z-index: -1;
	}

	.bg-contaniner::before {
		--size: 150vmax;
		content: "";
		inline-size: var(--size);
		block-size: var(--size);
		background-image: url("https://resource.tuniaokj.com/images/animate/animate3.png");
		background-size: var(--bg-size);
		background-repeat: repeat;
		transform: rotate(45deg);
		opacity: 0.25;
		animation: bg 6s linear infinite;
	}

	@media (prefers-reduced-motion: reduce) {
		.bg-contaniner::before {
			animation-duration: 0s;
		}
	}

	@keyframes bg {
		to {
			background-position: 0 calc(var(--bg-size) * -1);
		}
	}

	/* 移动背景部分 end*/

	/* 底部tabbar start*/
	.footerfixed {
		position: fixed;
		width: 100%;
		bottom: 0;
		z-index: 999;
		background-color: #FFFFFF;
		box-shadow: 0rpx 0rpx 30rpx 0rpx rgba(0, 0, 0, 0.07);
	}

	.tabbar {
		display: flex;
		align-items: center;
		min-height: 110rpx;
		justify-content: space-between;
		padding: 0;
		height: calc(110rpx + env(safe-area-inset-bottom) / 2);
		padding-bottom: calc(env(safe-area-inset-bottom) / 2);
	}

	.tabbar .action {
		font-size: 22rpx;
		position: relative;
		flex: 1;
		text-align: center;
		padding: 0;
		display: block;
		height: auto;
		line-height: 1;
		margin: 0;
		overflow: initial;
	}

	.tabbar .action .bar-icon {
		width: 100rpx;
		position: relative;
		display: block;
		height: auto;
		margin: 0 auto 10rpx;
		text-align: center;
		font-size: 42rpx;
		// line-height: 50rpx;
	}

	.tabbar .action .bar-icon image {
		width: 50rpx;
		height: 50rpx;
		display: inline-block;
	}

	.tabbar .action .bar-circle {
		position: relative;
		display: block;
		margin: -30rpx auto 20rpx;
		text-align: center;
		font-size: 52rpx;
		line-height: 90rpx;
		background-color: #FFCA28;
		width: 90rpx !important;
		height: 90rpx !important;
		overflow: hidden;
		border-radius: 50%;
		box-shadow: 0rpx 0rpx 20rpx 0rpx rgba(255, 202, 40, 0.5);
	}

	.tabbar .action .bar-circle image {
		width: 60rpx;
		height: 60rpx;
		display: inline-block;
		margin: 15rpx auto 15rpx;
	}
</style>