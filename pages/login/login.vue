<template>
	<view class="container">
		<view class="flex-center" style="height: 200rpx;margin-top: 200rpx;">
			<text v-if="register != 'account'" class="text-light" style="font-size: 50rpx">手机验证码登录</text>
			<text v-else class="text-light" style="font-size: 50rpx">账号密码登录</text>
		</view>
		<view class="px-4">
			<view v-if="register != 'account'" class="flex align-center">
				<text class="font-weight-bold">+86</text>
				<input
					type="number"
					v-model="form.phone"
					class="px-2 font rounded input text-light w-100"
					placeholder="手机号"
					placeholder-style="color: #212121"
					style="height: 100rpx;"
				/>
			</view>
			<view v-else class="flex align-center">
				<input
					type="text"
					v-model="form.username"
					class="px-2 font rounded input text-light w-100"
					placeholder="昵称/手机号/邮箱"
					placeholder-style="color: #212121"
					style="height: 100rpx;"
				/>
			</view>

			<divider class="mb-2"></divider>

			<view v-if="register != 'account'" class="flex align-center justify-between">
				<input
					type="number"
					v-model="form.code"
					class="px-2 font rounded input"
					placeholder="请输入验证码"
					placeholder-style="color: #212121"
					style="height: 100rpx;"
				/>
				<button
					v-if="!send"
					class="flex-center m-0"
					style="background-color: #eeeeee; height: 60rpx; font-size: 25rpx;"
					@click="sendCode"
				>
					获取验证码
				</button>
				<button
					v-if="send"
					class="flex-center m-0"
					style="background-color: #eeeeee; height: 60rpx; font-size: 25rpx;"
					@click="sendCode"
					:disabled="true"
				>
					还剩{{ sec }}s
				</button>
			</view>

			<view v-else class="flex align-center justify-between">
				<input
					type="password"
					v-model="form.password"
					class="px-2 font rounded input"
					placeholder="请输入密码"
					placeholder-style="color: #212121"
					style="height: 100rpx;"
				/>
				<button class="flex-center m-0" style="height: 60rpx; font-size: 25rpx;">忘记密码</button>
			</view>

			<divider></divider>
		</view>

		<view class="p-3 flex-center mt-5" @click="submit">
			<view class="bg-main rounded p-3 flex-center flex-1 rounded-circle" hover-class="bg-main-hover">
				<text class="text-white font-md">登 录</text>
			</view>
		</view>

		<view class="flex-center">
			<text class="font-sm" style="color: #0056B3;" @click="change">
				{{ register === 'account' ? '账号密码登录' : '验证码登录' }}
			</text>
			<text class="ml-2 mr-2" style="color: #424242;margin-top: -10rpx;">|</text>
			<text class="font-sm" style="color: #0056B3;">登录遇到问题</text>
		</view>

		<view class="flex-center mt-4">
			<view class="text-light-muted mt-2" style="height: 2rpx; width: 85rpx; background-color: #A9A5A0;"></view>
			<text class="font-sm text-light-muted ml-1 mr-1">社交账号登录</text>
			<view class="text-light-muted mt-2" style="height: 2rpx; width: 85rpx; background-color: #A9A5A0;"></view>
		</view>

		<view class="flex-center mt-4">
			<image
				style="width: 110rpx; height: 110rpx;"
				class="rounded-circle ml-5 mr-5"
				src="../../static/banner/wechat.png"
			></image>
			<image
				style="width: 110rpx; height: 110rpx;"
				class="rounded-circle ml-5 mr-5"
				src="../../static/banner/qq.png"
			></image>
			<image
				style="width: 110rpx; height: 110rpx;"
				class="rounded-circle ml-5 mr-5"
				src="../../static/banner/weibo.png"
			></image>
		</view>

		<view class="mt-4 flex-center">
			<text class="text-light-muted font-sm">注册及代表您同意</text>
			<text class="font-sm" style="color: #1890ff;">《社区直播协议》</text>
		</view>
	</view>
</template>

<script>
import divider from '../../components/common/divider.vue';
export default {
	components: {
		divider
	},
	data() {
		return {
			register: 'account',
			type: 'login',
			send: false,
			sec: 60,
			form: {
				username: '',
				password: '',
				phone: '',
				code: ''
			}
		};
	},
	methods: {
		change() {
			this.register = this.register === 'account' ? 'idcode' : 'account';
		},
		changeType() {
			this.type = this.type === 'login' ? 'phoneLogin' : 'login';
		},
		//登录
		submit() {
			let type = '';
			if (this.register === 'account') {
				type = 'login';
			} else {
				type = 'phoneLogin';
				this.form = {
					phone: this.form.phone,
					code: this.form.code
				};
			}
			console.log(type);
			console.log(this.form);
			this.$H.post('/' + type, this.form).then(res => {
				console.log(res);
				uni.showToast({
					title: '登录成功',
					icon: 'none'
				});
				this.$store.dispatch('login', res);
				uni.switchTab({
					url: '../my/my'
				});
			});
		},
		//发送验证码
		sendCode() {
			if (this.form.phone == '') {
				return uni.showToast({
					title: '请先输入手机号',
					icon: 'none'
				});
			} else if (this.form.phone.length != 11) {
				return uni.showToast({
					title: '请先输入正确的手机号',
					icon: 'none'
				});
			} else {
				this.send = true;
				this.$H.post('/sendcode', { phone: this.form.phone }).then(res => {
					// 倒计时60s结束后 可再次发送验证码
					let promise = new Promise((resolve, reject) => {
						let setTimer = setInterval(() => {
							this.sec = this.sec - 1;
							if (this.sec <= 0) {
								this.send = false;
								resolve(setTimer);
							}
						}, 1000);
					});
					promise.then(setTimer => {
						clearInterval(setTimer);
					});
					this.sec = 60;
				});
			}
		}
	}
};
</script>

<style scoped>
.container {
	width: 750rpx;
	height: 100vh;
	margin: 0;
	padding: 100rpx 0 0 0;
	background-size: cover;
	background-image: linear-gradient(to bottom, #ba7ace 0%, #8745ff 100%);
}
.input {
	border: none;
	background-color: transparent;
}
</style>
