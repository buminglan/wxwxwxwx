<template>
	<view>
		<my-login v-if="true"></my-login>
		
		<my-sess v-if="false"></my-sess>
	</view>
</template>

<script>
	import badgeMix from '@/mixins/tabber-badge.js'
	export default {
		mixins:[badgeMix],
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">

</style>
