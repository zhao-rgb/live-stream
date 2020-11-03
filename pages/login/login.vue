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
					type="text"
					v-model="form.username"
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
					type="text"
					v-model="form.password"
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
					type="text"
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
			<text class="ml-2 mr-2" style="color: #424242;">|</text>
			<text class="font-sm" style="color: #0056B3;">登录遇到问题</text>
		</view>

		<view class="flex-center mt-4">
			<view class="text-light-muted" style="height: 2rpx; width: 85rpx; background-color: #A9A5A0;"></view>
			<text class="font-sm text-light-muted ml-1 mr-1">社交账号登录</text>
			<view class="text-light-muted" style="height: 2rpx; width: 85rpx; background-color: #A9A5A0;"></view>
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
			<text class="font-sm" style="color: #1890ff;">《XXX社区协议》</text>
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
			sec: 10,
			form: {
				username: '',
				password: '',
				repassword: ''
			}
		};
	},
	methods: {
		change() {
			this.register = this.register === 'account' ? 'idcode' : 'account';
		},
		changeType() {
			this.type = this.type === 'login' ? 'reg' : 'login';
		},
		//登录
		submit() {
			let msg = this.type === 'login' ? '登录' : '注册';
			this.$H.post('/' + this.type, this.form).then(res => {
				console.log(res);
				uni.showToast({
					title: msg + '成功',
					icon: 'none'
				});
				if (this.type === 'reg') {
					this.changeType();
					this.form = {
						username: '',
						password: '',
						repassword: ''
					};
				} else {
					this.$store.dispatch('login', res);
				}
				uni.switchTab({
					url: '../index/index'
				});
			});
		},
		//发送验证码
		sendCode() {
			this.send = true;
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
			this.sec = 10;
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
