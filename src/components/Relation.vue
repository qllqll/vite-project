<template>
  <div>
    <div>【数据传递信息】</div>
    <div ref="target" class="h-full w-full">
    </div>
  </div>
</template>

<script setup>
import {ref, onMounted, watch} from 'vue'
import *as echarts from 'echarts'

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
    xAxis: {show: false, type: 'value'},
    yAxis: {show: false, type: 'value'},
    series: [
      {
        type: 'graph',
        layout: 'none',
        coordinateSystem: 'cartesian2d',
        symbolSize: 26,
        z: 3,
        edgeLabel: {
          normal: {
            show: true,
            color: '#fff',
            textStyle: {
              fontSize: 14
            },
            formatter: function (params) {
              return params.data.speed
            }
          }
        },
        label: {
          normal: {
            show: true,
            position: 'bottom',
            color: '#5e5e5e'
          }
        },
        edgeSymbol: ['none', 'arrow'],
        edgeSymbolSize: 8,
        data: props.data.relations.map(item => {
          if (item.id != 0) {
            return {
              name: item.name,
              category: 0,
              active: true,
              speed: `${item.speed}kb/s`,
              value: item.value
            }
          } else {
            return {
              name: item.name,
              value: item.value,
              symbolSize: 100,
              itemStyle: {
                normal: {
                  color: {
                    colorStops: [{
                      offset: 0,
                      color: '#157eff'
                    }, {
                      offset: 1,
                      color: '#35c2ff'
                    }]
                  }
                }
              },
              label:{
                normal:{
                  fontSize: 14
                }
              }

            }
          }
        }),
        links:props.data.relations.map((item,index)=>({
          source:item.source,
          target:item.target,
          speed:`${item.speed}kb/s`,
          lineStyle:{
            normal:{
              color:'#12b5d0',
              curveness:0.2
            }
          },
          label:{
            show:true,
            position: 'middle',
            offset:[10,0]
          }
        }))
      },
      {
        type:'lines',
        coordinateSystem: 'cartesian2d',
        z:1,
        effect:{
          show:true,
          smooth:false,
          trailLength:0,
          symbol:'arrow',
          color:'rgba(55,155,255,0.6)',
          symbolSize:12
        },
        lineStyle:{
          normal:{
            curveness: 0.2,
          }
        },
        data:[
          [{coord:[0,300]},{coord:[50,200]}],
          [{coord:[0,100]},{coord:[50,200]}],
          [{coord:[50,200]},{coord:[100,100]}],
          [{coord:[50,200]},{coord:[100,300]}],
        ]
      }
    ]
  }

  mChart.setOption(options)
};
watch(() => props.data, () => {
  renderChart()
})
</script>

<style scoped>

</style>
