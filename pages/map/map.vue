<template>
	<view>
        <!-- <web-view src="http://123.234.129.238:8001/SdForecast/MapSec/Map.html"></web-view> -->
		<!-- <web-view src="http://192.168.2.19/MapSec/Map.html"></web-view> -->
		<web-view v-if="url" :src="url"></web-view>
		<!-- <map style="width: 100%; height: 1200px;" :latitude="centerlati" :longitude="centerlongi" :scale="scale" :markers="markers"></map> -->
    </view>
</template>

<script>
	export default {
		data () {
			return {
				url: 'http://123.234.129.237:8002/SdForecast/MapSec/Map.html'
			}
		},
		methods: {
			refresh () {
				this.url = ''
				let timer = setTimeout(function () {
					this.url = 'http://123.234.129.237:8002/SdForecast/MapSec/Map.html'
					clearTimeout(timer)
				}.bind(this), 100)
			}
		},
		onNavigationBarButtonTap () {
			this.refresh()
		}
	}
</script>

