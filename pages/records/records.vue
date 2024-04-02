<template>
	<view style="padding: 20rpx;margin-top: 180rpx;">
		<!-- 顶部自定义导航 -->
		<tn-nav-bar fixed alpha customBack>
			<view slot="back" class='tn-custom-nav-bar__back' @click="goBack">
				<text class='icon tn-icon-left'></text>
				<text class='icon tn-icon-home-capsule-fill'></text>
			</view>
		</tn-nav-bar>
		<view class="box" v-for="item in recordList" :key="item.id" style="margin-bottom: 10rpx;">
			<view style="display: flex; margin-bottom: 10rpx;">
				<view>
					<uni-tag type="success" text="发放" size="mini" v-if="item.type === '发放'"></uni-tag>
					<uni-tag type="primary" text="公益" size="mini" v-if="item.type === '公益'"></uni-tag>
					<uni-tag type="error" text="服务" size="mini" v-if="item.type === '服务'"></uni-tag>
					<uni-tag type="warning" text="取消" size="mini" v-if="item.type === '取消'"></uni-tag>
					<text style="margin-left: 10rpx;">{{ item.content }}</text>
				</view>
				<view style="flex: 1; text-align: right; font-weight: bold;">
					<text style="color: red;" v-if="item.type === '发放'">+{{ item.money }}</text>
					<text style="color: dodgerblue;" v-if="item.type === '公益'">+{{ item.money }}</text>
					<text style="color: #888;" v-if="item.type === '服务'">-{{ item.money }}</text>
					<text style="color: orange;" v-if="item.type === '取消'">+{{ item.money }}</text>
				</view>
			</view>
			<view style="text-align: right; color: #888; font-size: 24rpx;">{{ item.time }}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				recordList: [],
				user: uni.getStorageSync('xm-user')
			}
		},
		onLoad() {
			this.load()
		},
		methods: {
			load() {
				this.$request.get('/records/selectAll', {
					userId: this.user.id
				}).then(res => {
					this.recordList = res.data || []
				})
			}
		},
		goBack() {
			setTimeout(() => {
				uni.switchTab({
					url: '/pages/meplus1/meplus1'
				})
			}, 500)
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