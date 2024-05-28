<script setup>
import HorizontalBar from './components/HorizontalBar.vue'
import RadarBar from './components/RadarBar.vue'
import Relation from './components/Relation.vue'
import TotalData from './components/TotalData.vue'
import MapChart from './components/MapChart.vue'
import VerticalBar from './components/VerticalBar.vue'
import RingBar from './components/RingBar.vue'
import WordCloud from './components/WordCloud.vue'

import { getVisualization} from './api/visualization.js'
import { ref } from 'vue'

const data = ref(null)
const loadData = async() => {
	data.value = await getVisualization()
	console.log(data.value)
}
loadData()

// setInterval(() => {
// 	loadData()
// }, 3000)
</script>

<template>
  <div class="bg-[url('assets/imgs/bg.jpg')] bg-cover bg-center h-screen text-white p-2 flex overflow-hidden">
	<div class="flex-1 mr-2 bg-opacity-50 bg-slate-800 p-3 flex flex-col">
		<HorizontalBar class="h-1/3 box-border pb-4" :data="data.regionData"/>
		<RadarBar class="h-1/3 box-border pb-4" :data="data.riskData"/>
		<Relation class="h-1/3" />
	</div>
	<div class="w-1/2 mr-2 flex flex-col">
		<TotalData class="bg-opacity-50 bg-slate-800 p-3" />
		<MapChart class="bg-opacity-50 bg-slate-800 p-3 mt-2 flex-1" />
	</div>
	<div class="flex-1 bg-opacity-50 bg-slate-800 p-3 flex flex-col">
		<VerticalBar class="h-1/3 box-border pb-4" :data="data.serverData"/>
		<RingBar class="h-1/3 box-border pb-4" />
		<WordCloud class="h-1/3 box-border" />
	</div>
	</div>
</template>

<style scoped>

</style>
