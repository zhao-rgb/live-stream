<template>
	<view>
		<image src="../../static/banner/3.jpg" style="width: 750rpx; height: 250rpx;"></image>
		<!-- 轮播图 -->
		<!-- <swiper
			:indicator-dots="true"
			:autoplay="true"
			:interval="3000"
			:duration="200"
			style="width: 750rpx; height: 250rpx;"
		>
			<swiper-item>
				<image src="../../static/banner/1.jpg" style="width: 750rpx; height: 250rpx;"></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/banner/2.jpg" style="width: 750rpx; height: 250rpx;"></image>
			</swiper-item>
		</swiper> -->

		<!-- 列表 -->
		<view class="flex flex-wrap">
			<f-list
				class="list-item"
				v-for="(item, index) in list"
				:key="index"
				:item="item"
				:index="index"
				@click="openLive"
			></f-list>
		</view>
		
	</view>
</template>

<script>
import fList from '@/components/f-list.vue';
export default {
	components: {
		fList
	},
	data() {
		return {
			list: [
				{
					name: '小圆圆',
					img: 'https://kkkksslls.oss-cn-beijing.aliyuncs.com/campus/短发46.jpg',
					money: 100,
					popular: 10,
					status: 0
				},
				{
					name: '果小果',
					img: 'https://kkkksslls.oss-cn-beijing.aliyuncs.com/campus/短发47.jpg',
					money: 30,
					popular: 8,
					status: 1
				},
				{
					name: '静静',
					img: 'https://kkkksslls.oss-cn-beijing.aliyuncs.com/campus/短发19.jpeg',
					money: 50,
					popular: 1,
					status: 0
				},
				{
					name: '肉肉',
					img: 'https://kkkksslls.oss-cn-beijing.aliyuncs.com/campus/短发18.jpeg',
					money: 60,
					popular: 1,
					status: 0
				},
				{
					name: '呆妹',
					img: 'https://kkkksslls.oss-cn-beijing.aliyuncs.com/campus/短发39.jpg',
					money: 70,
					popular: 1,
					status: 1
				},
				{
					name: '小天才',
					img: 'https://kkkksslls.oss-cn-beijing.aliyuncs.com/avatar/20200420202237.png',
					money: 10,
					popular: 292,
					status: 1
				},
				{
					name: '啦啦啦啦',
					img: 'https://kkkksslls.oss-cn-beijing.aliyuncs.com/avatar/20200420202118.png',
					money: 90,
					popular: 192,
					status: 1
				},
				{
					name: '撒野',
					img: 'https://kkkksslls.oss-cn-beijing.aliyuncs.com/avatar/20200420202138.png',
					money: 20,
					popular: 90,
					status: 1
				}
			]
		};
	},
	onLoad() {},
	methods: {
		openLive() {
			uni.navigateTo({
				url: '../live/live'
			});
		}
	}
};
</script>

<style>
.list-item {
	width: 375rpx;
	height: 375rpx;
	padding: 5rpx;
	box-sizing: border-box;
	position: relative;
}
</style>
