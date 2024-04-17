<template>
    <div>
        <div>【地区数据信息】</div>
        <div ref="target" class="w-full h-full"></div>
    </div>
</template>

<script lang='js' setup>
import { ref, onMounted } from 'vue'
import * as echarts from 'echarts'

// 组件通信传值
const props = defineProps({
    data: {
        type: Object,
        required: true
    }
})
console.log(props.data.regions)

// 1、初始化echarts图表
// 获取dom实例
const target = ref(null)
// echarts实例变量
let myChart = null

onMounted(() => {
    myChart = echarts.init(target.value) // 创建echarts实例
    myChart.setOption(options) // 3、通过实例.setOption(option)渲染图表
})

// 2、构建option配置对象
const options = {
    // x轴
    xAxis: {
        show: false,
        type: 'value',
        max: function(value) {
            return parseInt(value.max * 1.2)
        }
    },
    // y轴
    yAxis: {
        type: 'category',
        data: props.data.regions.map((item) => item.name),
        inverse: true,
        axisLine: {
            show: false
        },
        axisTick: {
            show: false
        },
        axisLabel: {
            color: '#9eb1c8'
        }
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
    // echarts网格绘制的位置，对应上、右、下、左
    grid: {
        top: 0,
        right: 0,
        bottom: 0,
        left: 0,
        containLabel: true // 计算边距时，包含标签
    },
    // 图表核心配置
    series: [
        {
            // 图表类型：柱状图
            type: 'bar',
            //数据筛选
            data: props.data.regions.map((item) => ({
                name: item.name,
                value: item.value
            })),
            // 显示背景
            showBackground: true,
            //背景颜色
            backgroundColor: {
                color: 'rbga(180, 180, 180, 0.2)'
            },
            // 轴的样式
            itemStyle: {
                color: '#5d98ce',       // 柱子的颜色
                barBorderRadius: 5,     // 柱子的圆角
                shadowColor: 'rbga(0, 0, 0, 0.2)',   // 柱子阴影颜色
                shadowBlur: 5           // 阴影模糊大小
            },
            // 轴宽度
            barWidth: 12,
            // 轴上的字
            label: {
                show: true,        // 显示
                position: 'right', // 显示位置：右侧
                // 标签文本颜色
                textStyle: {
                    color: '#fff',
                }
            }
        }
    ]
}
</script>

<style scoped>

</style>
