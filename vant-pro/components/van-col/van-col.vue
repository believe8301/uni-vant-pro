<template>
	<view class="custom-class" :class="[$u.bem('col', [span]), offset ? 'van-col--offset-' + offset : '']" :style="[rootStyle({ gutter })]"><slot /></view>
</template>

<script>
/**
 * @property {String | Number} offset 列元素偏移距离
 * @property {String | Number} span 列元素宽度
 */
export default {
	props: {
		span: String | Number,
		offset: String | Number
	},
	data() {
		return {
			gutter: null
		};
	},
	mounted() {
		uni.$on('getGutter', msg => {
			console.log(msg);
			this.gutter = msg;
		});
	},
	beforeDestroy() {
		uni.$off('getGutter');
	},
	methods: {
		rootStyle(data) {
			if (!data.gutter) {
				return '';
			}
			return this.$u.style({
				'padding-right': this.$u.addUnit(data.gutter / 2),
				'padding-left': this.$u.addUnit(data.gutter / 2)
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.van-col {
	float: left;
	background-clip: content-box;
	box-sizing: border-box;
}

.van-col--1 {
	width: 4.16666667%;
}

.van-col--offset-1 {
	margin-left: 4.16666667%;
}

.van-col--2 {
	width: 8.33333333%;
}

.van-col--offset-2 {
	margin-left: 8.33333333%;
}

.van-col--3 {
	width: 12.5%;
}

.van-col--offset-3 {
	margin-left: 12.5%;
}

.van-col--4 {
	width: 16.66666667%;
}

.van-col--offset-4 {
	margin-left: 16.66666667%;
}

.van-col--5 {
	width: 20.83333333%;
}

.van-col--offset-5 {
	margin-left: 20.83333333%;
}

.van-col--6 {
	width: 25%;
}

.van-col--offset-6 {
	margin-left: 25%;
}

.van-col--7 {
	width: 29.16666667%;
}

.van-col--offset-7 {
	margin-left: 29.16666667%;
}

.van-col--8 {
	width: 33.33333333%;
}

.van-col--offset-8 {
	margin-left: 33.33333333%;
}

.van-col--9 {
	width: 37.5%;
}

.van-col--offset-9 {
	margin-left: 37.5%;
}

.van-col--10 {
	width: 41.66666667%;
}

.van-col--offset-10 {
	margin-left: 41.66666667%;
}

.van-col--11 {
	width: 45.83333333%;
}

.van-col--offset-11 {
	margin-left: 45.83333333%;
}

.van-col--12 {
	width: 50%;
}

.van-col--offset-12 {
	margin-left: 50%;
}

.van-col--13 {
	width: 54.16666667%;
}

.van-col--offset-13 {
	margin-left: 54.16666667%;
}

.van-col--14 {
	width: 58.33333333%;
}

.van-col--offset-14 {
	margin-left: 58.33333333%;
}

.van-col--15 {
	width: 62.5%;
}

.van-col--offset-15 {
	margin-left: 62.5%;
}

.van-col--16 {
	width: 66.66666667%;
}

.van-col--offset-16 {
	margin-left: 66.66666667%;
}

.van-col--17 {
	width: 70.83333333%;
}

.van-col--offset-17 {
	margin-left: 70.83333333%;
}

.van-col--18 {
	width: 75%;
}

.van-col--offset-18 {
	margin-left: 75%;
}

.van-col--19 {
	width: 79.16666667%;
}

.van-col--offset-19 {
	margin-left: 79.16666667%;
}

.van-col--20 {
	width: 83.33333333%;
}

.van-col--offset-20 {
	margin-left: 83.33333333%;
}

.van-col--21 {
	width: 87.5%;
}

.van-col--offset-21 {
	margin-left: 87.5%;
}

.van-col--22 {
	width: 91.66666667%;
}

.van-col--offset-22 {
	margin-left: 91.66666667%;
}

.van-col--23 {
	width: 95.83333333%;
}

.van-col--offset-23 {
	margin-left: 95.83333333%;
}

.van-col--24 {
	width: 100%;
}

.van-col--offset-24 {
	margin-left: 100%;
}
</style>
