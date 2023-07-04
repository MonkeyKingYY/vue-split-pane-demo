<template>
	<div id="BottomTaskBar">
		<div class="img-contain">
			<img @click="leftClose" v-if="leftPercent !== 0" src="../assets/bottomTask/left_show.png" />
			<img @click="openLeft" v-if="leftPercent == 0" src="../assets/bottomTask/left_hidden.png" />
			<img @click="bottomClose" v-if="topPercent !== 100" src="../assets/bottomTask/bottom_show.png" />
			<img @click="openBottom" v-if="topPercent == 100" src="../assets/bottomTask/bottom_hidden.png" />
			<img @click="rightClose" v-if="rightPercent !== 100" src="../assets/bottomTask/right_show.png" style="margin-bottom: 1px" />
			<img @click="openRight" v-if="rightPercent == 100" src="../assets/bottomTask/right_hidden.png" />
		</div>
	</div>
</template>
<script>
export default {
	props: {
		leftPercent: {
			type: Number,
		},
		rightPercent: {
			type: Number,
		},
		topPercent: {
			type: Number,
		},
	},
	data() {
		return {}
	},
	methods: {
		leftClose() {
			this.$emit('leftClose')
		},
		rightClose() {
			this.$emit('rightClose')
		},
		bottomClose() {
			this.$emit('bottomClose')
		},
		openBottom() {
			this.$emit('openBottom')
		},
		openLeft() {
			this.$emit('openLeft')
		},
		openRight() {
			this.$emit('openRight')
		},
	},
}
</script>
<style scoped>
#BottomTaskBar {
	color: #fff;
	padding: 0 20px;
	display: flex;
	align-items: center;
	justify-content: flex-end;
}

#BottomTaskBar p {
	margin: 0;
	height: 25px;
	line-height: 25px;
}
#BottomTaskBar .img-contain img {
	width: 20px;
	height: 20px;
	cursor: pointer;
	margin-right: 10px;
}
</style>
