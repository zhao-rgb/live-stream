<template>
	<view>
		<view class="position-relative border-primary" style="width: 365rpx; height: 365rpx;" @click="openLive">
			<!-- 图片 -->
			<image
				:src="item.img"
				class="position-absolute"
				mode="aspectFill"
				style="width: 100%; height: 100%;"
			></image>
			<!-- 钱 -->
			<view
				class="rounded-circle px-2 flex align-center"
				style="position: absolute; left: 15rpx; top: 15rpx; background-color: rgba(0,0,0,0.4);"
			>
				<text class="iconfont iconbizhongguanli text-warning mr-1"></text>
				<text class="text-white font">{{ item.money }}</text>
			</view>

			<!-- 人气 -->
			<view
				class="rounded-circle px-2 flex align-center"
				style="position: absolute; right: 15rpx; top: 15rpx; background-color: rgba(0,0,0,0.4);"
			>
				<text class="font-sm text-white">人气：</text>
				<text class="text-white font">{{ item.popular }}</text>
			</view>

			<!-- 标题 -->
			<view
				class="rounded-circle px-2 flex align-center"
				style="position: absolute; left: 15rpx; bottom: 15rpx; background-color: rgba(0,0,0,0.4);"
			>
				<text class="text-white font">{{ item.name }}</text>
			</view>

			<!-- 是否结束 -->
			<view
				class="rounded-circle px-2 flex align-center"
				style="position: absolute; right: 15rpx; bottom: 15rpx; background-color: rgba(0,0,0,0.4);"
			>
				<text
					style="width: 20rpx; height: 20rpx;"
					:class="iconClass"
					class="rounded-circle mr-1"
				></text>
				<text class="text-white font-sm">{{ item.status === 0 ? '直播中' : '已结束' }}</text>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	props: {
		item: Object,
		index: [Number, String]
	},
	methods: {
		openLive() {
			this.$emit('click', {
				index: this.index
			});
		}
	},
	computed: {
		iconClass() {
			let color = this.item.status === 0 ? 'bg-success' : 'bg-danger';
			return color;
		}
	}
};
</script>

<style></style>
