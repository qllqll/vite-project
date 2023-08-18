<template>

  <div>
    <div>【云端报警风险】</div>
    <div ref="target" class="h-full w-full">
    </div>
  </div>

</template>

<script setup>
import {ref, onMounted, watch} from 'vue'
import * as echarts from "echarts";

const props = defineProps({
  data: {
    type: Object,
    require: true
  }
})

const target = ref(null)
let mChart = null

onMounted(() => {
  mChart = echarts.init(target.value)
  renderChart()
})
const renderChart = () => {
  const options = {
    // 雷达图的坐标系配置
    radar: {
      name: {
        textStyle: {
          color: '#05d5ff',
          fontSize: 14
        }
      },
      shape: 'polygon',
      center: ['50%', '50%'],
      radius: '80%',
      startAngle: 120,
      axisLine: {
        lineStyle: {
          color: 'rgba(5,213,255,.8)'
        }
      },
      splitLine: {
        show: true,
        lineStyle: {
          width: 1,
          color: 'rgba(5,213,255,.8)'
        }
      },
      indicator: props.data.risks.map((item) => ({
        name: item.name,
        max: 100
      })),
      splitArea: false
    },
    // 坐标几点
    polar: {
      center: ['50%', '50%'],
      radius: '0%'
    },
    //   坐标角度
    angleAxis: {
      min: 0,
      interval: 5,
      clockwise: false,
      splitLine: {show: false}
    },
    // 径向轴
    radiusAxis: {
      min: 0,
      interval: 20,
      splitLine: {show: true}
    },
    // 图标核心配置
    series: {
      type:'radar',
      symbol:'circle',
      symbolSize:10,
      itemStyle:{
        normal:{
          color:'#05b5ff'
        }
      },
      areaStyle:{
        normal:{
          color:'#05d5ff',
          opacity:0.5
        }
      },
      lineStyle:{
        width: 2,
        color:'#05d5ff'
      },
      label:{
        normal:{
          show:true,
          color:'#fff'
        }
      },
      data:[{
        value:props.data.risks.map((item)=>item.value)
      }]
    }
  }
  mChart.setOption(options)

}
watch(() => props.data, () => {
  renderChart()
})
</script>

<style scoped>

</style>
