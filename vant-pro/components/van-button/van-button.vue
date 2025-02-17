<template>
	<button
		:id="id"
		class="custom-class"
		:class="
			[
				hairline ? 'van-hairline--surround' : '',
				$u.bem('button', [type, size, { block, round, plain, square, loading, disabled, hairline, unclickable: disabled || loading }])
			]
		"
		hover-class="van-button--active hover-class"
		:lang="lang"
		:form-type="formType"
		:style="[rootStyle()]"
		:open-type="disabled || loading || (canIUseGetUserProfile && openType === 'getUserInfo') ? '' : openType"
		:business-id="businessId"
		:session-from="sessionFrom"
		:send-message-title="sendMessageTitle"
		:send-message-path="sendMessagePath"
		:send-message-img="sendMessageImg"
		:show-message-card="showMessageCard"
		:app-parameter="appParameter"
		:aria-label="ariaLabel"
		:bindtap="disabled || loading ? '' : 'onClick'"
		bindgetuserinfo="onGetUserInfo"
		bindcontact="onContact"
		bindgetphonenumber="onGetPhoneNumber"
		binderror="onError"
		bindlaunchapp="onLaunchApp"
		bindopensetting="onOpenSetting"
	>
		<block v-if="loading">
			<van-loading custom-class="loading-class" :size="loadingSize" :type="loadingType" :color="loadingColor()" />
			<view v-if="loadingText" class="van-button__loading-text">{{ loadingText }}</view>
		</block>
		<block v-else>
			<van-icon v-if="icon" size="1.2em" :name="icon" :class-prefix="classPrefix" class="van-button__icon" :custom-style="{'line-height': 'inherit'}" />
			<view class="van-button__text"><slot /></view>
		</block>
	</button>
</template>

<script>
import { button } from '../../libs/minixs/button.js';
/**
 * @property {String} type 按钮类型，可选值为 primary info warning danger
 * @property {String} size 按钮尺寸，可选值为 normal large small mini
 * @property {String} color 按钮颜色，支持传入linear-gradient渐变色
 * @property {String} icon 左侧图标名称或图片链接
 * @property {Boolean} class-prefix 图标类名前缀，同 Icon 组件的 class-prefix 属性
 * @property {Boolean} plain 是否为朴素按钮
 * @property {Boolean} block 是否为块级元素	
 * @property {Boolean} round 是否为圆形按钮	
 * @property {Boolean} square 是否为方形按钮
 * @property {Boolean} disabled 是否禁用按钮
 * @property {Boolean} hairline 是否使用 0.5px 边框
 * @property {Boolean} loading 是否显示为加载状态
 * @property {String} loading-type 加载状态图标类型，可选值为 circular|spinner
 * @property {Object} loading-text 加载状态提示文字	
 * @property {String} loading-size 加载图标大小	
 * @property {Object} custom-style 自定义样式
 * @property {String} form-type 用于 form 组件，可选值为submit reset，点击分别会触发 form 组件的 submit/reset 事件
 */
export default {
	mixins: [button],
	data() {
		return {
			baseStyle: ''
		};
	},
	props: {
		formType: {
			type: String,
			default: ''
		},
		icon: {
			type: String,
			default: ''
		},
		classPrefix: {
			type: String,
			default: 'van-icon'
		},
		plain: Boolean,
		block: Boolean,
		round: Boolean,
		square: Boolean,
		loading: Boolean,
		hairline: Boolean,
		disabled: Boolean,
		loadingText: String,
		customStyle: Object,
		loadingType: {
			type: String,
			default: 'circular'
		},
		// 类型 primary success warning danger
		type: {
			type: String,
			default: 'default'
		},
		// 图标尺寸 large small mini
		size: {
			type: String,
			default: 'normal'
		},
		loadingSize: {
			type: String,
			default: '20rpx'
		},
		// 按钮颜色，支持传入 linear-gradient 渐变色
		color: String
	},
	methods: {
		onClick(event) {
			this.$emit('click', event);
			const { canIUseGetUserProfile, openType, getUserProfileDesc, lang } = this.data;
			if (openType === 'getUserInfo' && canIUseGetUserProfile) {
				wx.getUserProfile({
					desc: getUserProfileDesc || '  ',
					lang: lang || 'en',
					complete: userProfile => {
						this.$emit('getuserinfo', userProfile);
					}
				});
			}
		},
		rootStyle() {
			if (!this.color) {
				return this.customStyle;
			}

			var properties = {
				color: this.plain ? this.color : '#fff',
				background: this.plain ? null : this.color
			};

			// hide border when color is linear-gradient
			if (this.color.indexOf('gradient') !== -1) {
				properties.border = 0;
			} else {
				properties['border-color'] = this.color;
			}

			return this.$u.style(properties, this.customStyle);
		},
		loadingColor() {
			if (this.plain) {
				return this.color ? this.color : '#c9c9c9';
			}

			if (this.type === 'default') {
				return '#c9c9c9';
			}

			return '#fff';
		}
	}
};
</script>

<style scoped lang="scss">
.van-button {
	position: relative;
	display: inline-flex;
	align-items: center;
	justify-content: center;
	box-sizing: border-box;
	padding: 0;
	text-align: center;
	vertical-align: middle;
	-webkit-appearance: none;
	-webkit-text-size-adjust: 100%;
	height: $button-default-height;
	line-height: $button-line-height;
	font-size: $button-default-font-size;
	transition: opacity $animation-duration-fast;
	border-radius: $button-border-radius;
}

.van-button:before {
	position: absolute;
	top: 50%;
	left: 50%;
	width: 100%;
	height: 100%;
	border: inherit;
	border-radius: inherit;
	transform: translate(-50%, -50%);
	opacity: 0;
	content: ' ';
	background-color: $black;
	border-color: $black;
}

.van-button:after {
	border-width: 0;
}

.van-button--active:before {
	opacity: 0.15;
}

.van-button--unclickable:after {
	display: none;
}

.van-button--default {
	color: $button-default-color;
	background: $button-default-background-color;
	border: $button-border-width solid $button-default-border-color;
}

.van-button--primary {
	color: $button-primary-color;
	background: $button-primary-background-color;
	border: $button-border-width solid $button-primary-border-color;
}

.van-button--info {
	color: $button-info-color;
	background: $button-info-background-color;
	border: $button-border-width solid $button-info-border-color;
}

.van-button--danger {
	color: $button-danger-color;
	background: $button-danger-background-color;
	border: $button-border-width solid $button-danger-border-color;
}

.van-button--warning {
	color: $button-warning-color;
	background: $button-warning-background-color;
	border: $button-border-width solid $button-warning-border-color;
}

.van-button--plain {
	background: $button-plain-background-color;
}

.van-button--plain.van-button--primary {
	color: $button-primary-background-color;
}

.van-button--plain.van-button--info {
	color: $button-info-background-color;
}

.van-button--plain.van-button--danger {
	color: $button-danger-background-color;
}

.van-button--plain.van-button--warning {
	color: $button-warning-background-color;
}

.van-button--large {
	width: 100%;
	height: $button-large-height;
}

.van-button--normal {
	padding: 0 30rpx;
	font-size: $button-normal-font-size;
}

.van-button--small {
	min-width: $button-small-min-width;
	height: $button-small-height;
	padding: 0 $padding-xs;
	font-size: $button-small-font-size;
}

.van-button--mini {
	display: inline-block;
	min-width: $button-mini-min-width;
	height: $button-mini-height;
	font-size: $button-mini-font-size;
}

.van-button--mini + .van-button--mini {
	margin-left: 10rpx;
}

.van-button--block {
	display: flex;
	width: 100%;
}

.van-button--round {
	border-radius: $button-round-border-radius;
}

.van-button--square {
	border-radius: 0;
}

.van-button--disabled {
	opacity: $button-disabled-opacity;
}

.van-button__text {
	display: inline;
}

.van-button__icon + .van-button__text:not(:empty),
.van-button__loading-text {
	margin-left: 8rpx;
}

.van-button__icon {
	min-width: 1em;
	line-height: inherit !important;
	vertical-align: top;
}

.van-button--hairline {
	padding-top: 1rpx;
	border-width: 0;
}

.van-button--hairline:after {
	border-color: inherit;
	border-width: 1rpx;
	border-radius: calc($button-border-radius * 2);
}

.van-button--hairline.van-button--round:after {
	border-radius: $button-round-border-radius;
}

.van-button--hairline.van-button--square:after {
	border-radius: 0;
}
</style>
