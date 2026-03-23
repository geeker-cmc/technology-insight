<script setup lang="ts">
import * as echarts from "echarts"
import { onBeforeUnmount, onMounted, ref, watch } from "vue"

const props = defineProps<{
  xAxisData: string[]
  trendData: number[]
}>()

const chartRef = ref<HTMLElement>()
let chartInstance: echarts.ECharts | null = null

function resizeChart() {
  chartInstance?.resize()
}

onMounted(() => {
  if (chartRef.value) {
    chartInstance = echarts.init(chartRef.value)

    const option = {
      grid: { left: 40, right: 10, top: 10, bottom: 20 },
      tooltip: {
        trigger: "axis"
      },
      xAxis: {
        type: "category",
        data: props.xAxisData,
        axisLine: { show: false },
        axisTick: { show: false },
        axisLabel: { color: "#999", fontSize: 10 }
      },
      yAxis: {
        type: "value",
        splitLine: {
          show: true,
          lineStyle: { type: "dashed", color: "#eee" }
        },
        axisLabel: {
          color: "#999",
          fontSize: 10,
          formatter: (value: number) => value >= 1000 ? `${value / 1000}K` : value
        }
      },
      series: [
        {
          data: props.trendData,
          type: "line",
          smooth: true,
          showSymbol: false,
          symbolSize: 6,
          itemStyle: {
            color: "#000",
            borderColor: "#fff",
            borderWidth: 2
          },
          lineStyle: {
            color: "#00d27a",
            width: 3
          },
          areaStyle: {
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              { offset: 0, color: "rgba(0, 210, 122, 0.4)" },
              { offset: 1, color: "rgba(0, 210, 122, 0)" }
            ])
          },
          markPoint: {
            data: [
              { type: "max", symbol: "circle", symbolSize: 6, itemStyle: { color: "#222" } }
            ],
            label: { show: false }
          }
        }
      ]
    }
    chartInstance.setOption(option)
    window.addEventListener("resize", resizeChart)
  }
})

onBeforeUnmount(() => {
  window.removeEventListener("resize", resizeChart)
  chartInstance?.dispose()
})

watch(() => [props.xAxisData, props.trendData], () => {
  if (chartInstance) {
    chartInstance.setOption({
      xAxis: { data: props.xAxisData },
      series: [{ data: props.trendData }]
    })
  }
}, { deep: true })
</script>

<template>
  <div ref="chartRef" class="w-full h-full min-h-160px" />
</template>
