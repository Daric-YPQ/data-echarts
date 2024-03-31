<template>
    <div>
        <div>【地区数据信息】</div>
        <div ref="target" class="w-full h-full">
        </div>
    </div>
</template>

<script lang='js' setup>
import { ref, onMounted } from 'vue'
import * as echarts from 'echarts'

const props = defineProps({
    data: {
        type: Object,
        required: true
    }
})
// console.log(props.data)

// 1、初始化echarts图表
let myChart = null
const target = ref(null)
onMounted(() => {
    myChart = echarts.init(target.value) // 创建echarts实例
    renderChart()
})

// 2、构建option配置对象
const renderChart = () => {
    const options = {
        xAxis: {
            show: false,
            type: 'value',
            max: function(value) {
                return parseInt(value.max * 1.2)
            }
        },
        yAxis: {
            type: 'category',
            data: props.data.regions.map( (item) => item.name ),
            inverse: true,
            axisLine: {
                show: false
            },
            axisTick: {
                show: false
            },
            axisLable: {
                color: '#9eb1c8'
            }
        },
        grid: {
            top: 0,
            right: 0,
            bottom: 0,
            left: 0,
            containLable: true // 计算位置的时候，将标签作为图表整体去设置位置
        },
        // 核心配置
        series: [
            {
                type: 'bar',
                data: props.data.regions.map( (item) => ({
                    name: item.name,
                    value: item.value
                })),
                showBackground: true,
                backgroundColor: {
                    color: 'rbga(180, 180, 180, 0.2)'
                },
                itemStyle: {
                    color: '#5d98ce',
                    barBorderRadius: 5,
                    shadowColor: 'rbga(0, 0, 0, 0.2)',
                    shadowBlur: 5
                },
                barWidth: 12,
                lable: {
                    show: true,
                    position: 'right',
                    textStyle: {
                        color: '#fff',
                    }
                }
            }
        ]
    }

    // 3、通过实例.setOption(option)渲染图表
    myChart.setOption(options)
}
</script>

<style scoped>

</style>
