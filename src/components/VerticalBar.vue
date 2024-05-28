<template>
    <div>
        <div>【服务资源占用比】</div>
        <div ref="target" class="w-full h-full"></div>
    </div>
</template>

<script lang='js' setup>
import { onMounted, ref, watch } from 'vue'
import * as echarts from 'echarts'

const props = defineProps({
	data: {
		type: Object,
		required: true
	}
})

const target = ref(null)
let myChart = null
onMounted(() => {
	myChart = echarts.init(target.value)
	myChart.setOption(options)
})

const options = {
    // X轴
    xAxis: {
        type: 'category',
        // 根据服务端数据筛选
        data: props.data.servers.map((item) => item.name),
        // 文字色值
        axisLabel: {
            color: '#9EB1C8'
        }
    },
    // Y轴
    yAxis: {
        // 数据展示
        type: 'value',
        // 不显示轴
        show: false,
        // 最大值（防止触顶）
        max: function (value) {
            // 取整
            return parseInt(value.max * 1.2)
        }
    },
    // echarts 网格绘制的位置，对应 上、右、下、左
    grid: {
        top: 16,
        right: 0,
        bottom: 26,
        left: -26,
        // 计算边距时，包含标签
        containLabel: true
    },
    // 提示信息
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'shadow'
        },
        backgroundColor: '#fff', // 悬浮框背景色
        borderColor: '#000', // 悬浮框边框颜色
        borderWidth: 1, // 悬浮框边框宽度
        textStyle: { // 悬浮框文字样式
            color: '#000',
            fontSize: 12
        }
    },
    // 柱形图核心配置
    series: {
        // 柱形图
        type: 'bar',
        // 数据筛选
        data: props.data.servers.map((item) => ({
            name: item.name,
            value: item.value
        })),
        // 每个轴的样式
        itemStyle: {
            color: '#479AD3', // 设置柱子的颜色
            barBorderRadius: 5, // 设置柱子的圆角
            shadowColor: 'rgba(0, 0, 0, 0.3)', // 设置柱子的阴影颜色
            shadowBlur: 5 // 设置柱子的阴影模糊大小
        },
        // 柱子宽度
        barWidth: 12,
        // 文本
        label: {
            show: true,
            // 设置标签位置为右侧
            position: 'right',
            textStyle: {
                // 设置标签文本颜色
                color: '#ffb'
            },
            formatter: '{c}%'
        }
    }
}
</script>

<style scoped>

</style>
