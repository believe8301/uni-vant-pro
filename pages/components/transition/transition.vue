<template>
	<view class="content">
		<view class="result-div">
			<view class="headline">结果展示</view>
			<view class="result-content">
				<van-transition
					v-model="selectState"
					:duration="selectDuration"
					:name="selectAnimation"
					:customStyle="selectStyle"
					@before-enter="beforeEnter"
					@enter="enter"
					@after-enter="afterEnter"
					@before-leave="beforeLeave"
					@leave="leave"
					@after-leave="afterLeave"
				>
					<view class="transition-content"></view>
				</van-transition>
			</view>
		</view>
		<view class="condition-div">
			<view class="condition-title">状态</view>
			<view class="condition-content">
				<view class="condition-unit" :class="{ 'select-unit': selectState === item.type }" v-for="(item, index) in stateType" :key="index" @click="chooseState(item.type)">
					{{ item.mode }}
				</view>
			</view>
		</view>
		<view class="condition-div">
			<view class="condition-title">动画类型</view>
			<view class="condition-content">
				<view
					class="condition-unit"
					:class="{ 'select-unit': selectAnimation === item.type }"
					v-for="(item, index) in animationType"
					:key="index"
					@click="chooseAnimation(item.type)"
				>
					{{ item.mode }}
				</view>
			</view>
		</view>

		<view class="condition-div">
			<view class="condition-title">动画时长</view>
			<view class="condition-content">
				<view
					class="condition-unit"
					:class="{ 'select-unit': selectDuration === item.type }"
					v-for="(item, index) in durationType"
					:key="index"
					@click="chooseDuration(item.type)"
				>
					{{ item.mode }}
				</view>
			</view>
		</view>
		<view class="condition-div">
			<view class="condition-title">自定义样式</view>
			<view class="condition-content">
				<view class="condition-unit" :class="{ 'select-unit': item.state }" v-for="(item, index) in styleType" :key="index" @click="chooseStyle(item)">
					{{ item.mode }}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			stateType: [{ type: false, mode: '隐藏' }, { type: true, mode: '显示' }],
			selectState: false,
			animationType: [
				{ type: 'fade', mode: '默认' },
				{ type: 'slide-up', mode: '上滑' },
				{ type: 'slide-down', mode: '下滑' },
				{ type: 'slide-left', mode: '左滑' },
				{ type: 'slide-right', mode: '右滑' }
				// { type: 'fade-up', mode: '上滑淡入' },
				// { type: 'fade-down', mode: '下滑淡入' },
				// { type: 'fade-left', mode: '左滑淡入' },
				// { type: 'fade-right', mode: '右滑淡入' }
			],
			selectAnimation: 'fade',
			durationType: [{ type: 300, mode: '默认' }, { type: 1000, mode: '1000' }],
			selectDuration: 300,
			styleType: [{ type: {}, state: true, mode: '默认' }, { type: { borderRadius: '18rpx', overflow: 'hidden', display: 'flex' }, state: false, mode: '自定义样式' }],
			selectStyle: {}
		};
	},
	onLoad() {},
	methods: {
		/**
		 * @param {Object} type
		 * 是否显示组件
		 */
		chooseState(type) {
			this.selectState = type;
		},
		/**
		 * 选择动画类型
		 */
		chooseAnimation(type) {
			this.selectAnimation = type;
		},
		/**
		 * @param {Object} type
		 * 选择动画时长
		 */
		chooseDuration(type) {
			this.selectDuration = type;
		},
		/**
		 * @param {Object} type
		 * 自定义样式
		 */
		chooseStyle(item) {
			this.$set(this.styleType.filter(el => el.state)[0], 'state', false);
			this.$set(item, 'state', true);
			this.selectStyle = item.type;
		},
		/**
		 * 进入前
		 */
		beforeEnter() {
			console.log('进入前');
		},
		/**
		 * 进入中
		 */
		enter() {
			console.log('进入中');
		},
		/**
		 * 进入后
		 */
		afterEnter() {
			console.log('进入后');
		},
		/**
		 * 离开前
		 */
		beforeLeave() {
			console.log('离开前');
		},
		/**
		 * 离开
		 */
		leave() {
			console.log('离开中');
		},
		/**
		 * 离开后
		 */
		afterLeave() {
			console.log('离开后');
		}
	}
};
</script>

<style lang="scss" scoped>
.content {
	padding: 32rpx;
	.result-div {
		background-color: $gray-1;
		border: 1rpx dotted $gray-5;
		border-radius: 16rpx;
		padding: 32rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		.headline {
			font-size: 32rpx;
			color: #333333;
			font-weight: bold;
			padding-bottom: 32rpx;
		}
		.result-content {
			height: 200rpx;
			width: 200rpx;
			overflow: hidden;
			.transition-content {
				height: 200rpx;
				width: 200rpx;
				background-color: $blue;
			}
		}
	}
	.condition-div {
		.condition-title {
			padding: 32rpx 24rpx;
			font-size: 32rpx;
			color: #333333;
			font-weight: bold;
		}
		.condition-content {
			background-color: $gray-1;
			border-radius: 8rpx;
			padding: 16rpx;
			margin: 24rpx;
			display: flex;
			justify-content: space-between;
			.condition-unit {
				width: 100%;
				padding: 0 32rpx;
				font-size: 28rpx;
				color: #333333;
				border-radius: 8rpx;
				text-align: center;
			}
			.select-unit {
				background-color: $white;
			}
		}
	}
}
</style>
