<!-- 漏斗图 -->
<template>
  <div class="funnel-container">
    <div class="table">
      <div class="tr bold">
        <div class="td">事件</div>
        <div class="td">次数</div>
      </div>
      <div class="tr" v-for="item in funnelData">
        <div class="td">
          <input class="name" v-model="item.name">
        </div>
        <div class="td">
          <input class="value" v-model="item.value">
        </div>
      </div>
    </div>
    <!-- <div class="table">
      <div class="tr bold">
        <div class="td">事件</div>
        <div class="td">次数</div>
      </div>
      <div class="tr" v-for="item in funnelData">
        <div class="td">{{ item.name }}</div>
        <div class="td">{{ item.value }}</div>
      </div>
    </div> -->
    <div ref="funnelChart" class="funnel-chart"></div>
  </div>

</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';

import * as echarts from 'echarts';

const funnelChart = ref(null);
const funnelData = ref([
  { value: 128, name: "登录" },
  // { value: 25, name: "首页" },
  { value: 35, name: "基金列表" },
  { value: 16, name: "基金详情" },
  { value: 4, name: "申购弹窗" },
  { value: 1, name: "确认申购" },
  { value: 1, name: "成功下单" },  
]);

watch(funnelData, () => {
  initFunnelChart();
}, { deep: true });

const initFunnelChart = () => {
  if (funnelChart.value) {
    const chart = echarts.init(funnelChart.value);
    chart.setOption({
      title: {
        text: 'webportal 交易转换率',
        textStyle: {
          color: '#333'
        },
        left: 'center',
        top: 'top'
      },
      series: [{  
        type: 'funnel', 
        data: funnelData.value,
        label: {
          show: true,
          position: 'right',
          formatter: (params: { value: any; }) => {
            const total = funnelData.value[0].value
            const value = params.value
            const percent = ((value / total) * 100).toFixed(2)
            return `${percent}%`;
          }
        },
        labelLine: {  
          show: false,  
        },
        itemStyle: {
          borderRadius: 10,
          borderColor: '#fff',
          borderWidth: 2
        },
        emphasis: {   
          label: {  
            show: true,
            position: 'inside',
            formatter: (params: { value: any; }) => {
              const total = funnelData.value[0].value
              const value = params.value
              const percent = ((value / total) * 100).toFixed(2)
              return `${percent}%`;
            },
            fontSize: 20,
            fontWeight: 'bold',
            color: '#333'
          }
        },
        animationDuration: 1000,
        animationDurationUpdate: 1000, 
      }],
      // 图例
      legend: {
        data: funnelData.value.map(item => item.name),
        // 图例位置, 底部
        orient: 'horizontal',
        left: 'center',
        top: 'bottom', 
        textStyle: {
          color: '#333',
          fontSize: 14,
        }
      }
    });
  }
};

onMounted(() => {
  initFunnelChart();
});
</script>

<style  scoped>
.funnel-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
.table {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.bold {
  font-weight: bold;
}
.tr {
  display: flex;
  flex-direction: row;
} 
.td {
  width: 100px;
  height: 30px;
  background-color: #f0f0f0;
  text-align: center;
  line-height: 30px;
  border: 1px solid #ccc;
  input {
    width: 100px;
    border: none;
    outline: none;
    background: transparent;
    margin: 0;
    padding: 0;
    text-align: center;
  }
}
.funnel-chart {
  width: 480px;
  height: 480px;
  margin-left: 20px;
}
</style>


