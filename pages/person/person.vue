<template>
	<view style="padding: 20rpx;margin-top: 180rpx;">
		<!-- 顶部自定义导航 -->
		<tn-nav-bar fixed alpha customBack>
			<view slot="back" class='tn-custom-nav-bar__back' @click="goBack()">
				<text class='icon tn-icon-left'></text>
				<text class='icon tn-icon-home-capsule-fill'></text>
			</view>
		</tn-nav-bar>
		<view class="box" style="padding: 40rpx 20rpx;">
			<uni-forms :modelValue="form" :rules="rules" ref="formRef" label-width="140rpx" label-align="right">
				<uni-forms-item label="头像" name="avatar">
					<uni-file-picker limit="1" :image-styles="imageStyles" :del-icon="false" :disable-preview="true"
						fileMediatype="image" v-model="imgs" @select="handleImgUploadSuccess"></uni-file-picker>

				</uni-forms-item>
				<uni-forms-item label="账号" name="username">
					<uni-easyinput type="text" v-model="form.username" placeholder="" disabled />
				</uni-forms-item>
				<uni-forms-item label="密码" name="password" required>
					<uni-easyinput type="password" v-model="form.password" placeholder="请输入密码" />
				</uni-forms-item>
				<uni-forms-item label="姓名" name="name" required>
					<uni-easyinput type="text" v-model="form.name" placeholder="请输入姓名" />
				</uni-forms-item>
				<uni-forms-item label="性别" name="sex">
					<uni-data-checkbox style="position: relative; top: 10rpx;" v-model="form.sex"
						:localdata="range"></uni-data-checkbox>
				</uni-forms-item>
				<uni-forms-item label="电话" name="phone">
					<uni-easyinput type="text" v-model="form.phone" placeholder="请输入电话" />
				</uni-forms-item>

				<view>
					<button type="primary" @click="save" class="my-button-primary">保 存</button>
				</view>
			</uni-forms>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form: {},
				rules: {
					password: {
						rules: [{
							required: true,
							errorMessage: '请填写密码',
						}]
					},
					name: {
						rules: [{
							required: true,
							errorMessage: '请填写名称',
						}]
					},
				},
				imgs: [],
				imageStyles: {
					"height": 80, // 边框高度
					"width": 80, // 边框宽度
					"border": { // 如果为 Boolean 值，可以控制边框显示与否
						"color": "#eee", // 边框颜色
						"width": "1px", // 边框宽度
						"style": "solid", // 边框样式
						"radius": "50%" // 边框圆角，支持百分比
					}
				},
				range: [{
						text: '男',
						value: '男'
					},
					{
						text: '女',
						value: '女'
					},
				]
			}
		},
		onShow() {
			this.form = uni.getStorageSync('xm-user')
			if (this.form.avatar) {
				this.imgs[0] = {
					url: this.form.avatar
				}
			}

		},
		methods: {
			save() {
				this.$request.put('/user/update', this.form).then(res => {
					if (res.code === '200') {
						uni.showToast({
							icon: 'success',
							title: '操作成功'
						})
						uni.setStorageSync('xm-user', this.form)
					} else {
						uni.showToast({
							icon: 'none',
							title: res.msg
						})
					}
				})
			},
			handleImgUploadSuccess(e) {
				let _this = this
				const filePath = e.tempFilePaths[0]
				uni.uploadFile({
					url: _this.$baseUrl + '/files/upload', //自己的后端接口（默认发送post请求） 注意 _this.$baseUrl需要在全局变量定义
					filePath: filePath,
					name: "file", //这里应为自己后端文件形参的名字
					success(res) {
						let url = JSON.parse(res.data || '{}').data // 获取返回的图像链接
						_this.form.avatar = url // 给表单图像属性赋值
					}
				})
			},
			goBack() {
				setTimeout(() => {
					uni.switchTab({
						url: '/pages/meplus1/meplus1'
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