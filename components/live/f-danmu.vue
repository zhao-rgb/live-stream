<template>
	<view style="position: fixed; bottom: 120rpx; left: 0; right: 0;">
		<!-- 使用动画纵向滚动，将scrollInToView方法的返回值绑定到scroll-into-view属性上 -->
		<scroll-view
			scroll-y="true"
			style="width: 520rpx; height: 300rpx;"
			scroll-with-animation
			class="pl-3"
			:scroll-into-view="scrollInToView"
		>
			<view
				:id="'danmu' + item.id"
				class="flex justify-start align-center rounded p-2 mb-2"
				style="background-color: rgba(255,255,255,0.125);"
				v-for="(item, index) in list"
				:key="index"
			>
				<text class="font-md text-danger">{{ item.name }}:</text>
				<text class="font-md text-white">{{ item.content }}</text>
			</view>
		</scroll-view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			scrollInToView: '', //滚动到哪个元素的view视图
			list: [] //弹幕数组
		};
	},
	mounted() {
		// let id = 1
		// setInterval(() =>{
		//   this.list.push({
		//     id:id,
		//     name: '观众'+id,
		//     content: '发言_'+id
		//   })
		//   this.toBottom()
		//   id++
		// },2000)
	},
	methods: {
		// 发送弹幕
		send(data) {
			this.list.push(data);
			// 置于底部
			this.toBottom();
			// 自动消失
			this.autoHide();
		},
		toBottom() {
			setTimeout(() => {
				let len = this.list.length;
				if (len > 0 && this.list[len - 1]) {
					this.scrollInToView = 'danmu' + this.list[len - 1].id;
				}
			}, 200);
		},
		autoHide() {
			//定时器，5秒内从数组移除掉，思考：移除了哪个？
			if (this.list.length) {
				let timer = setTimeout(() => {
					this.list.splice(0, 1);
				}, 5000);
			}
		},
	}
};
</script>

<style></style>
