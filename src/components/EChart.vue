<template>
  <div>
    <div id="main" style="width: 100vw; height: 100vh"></div>
    <div id="main1" style="width: 100vw; height: 100vh"></div>
  </div>
</template>

<script>
export default {
  name: "ECharts",
  methods: {
    drawBarChart() {
      //2. 基于准备好的dom，初始化echarts实例
      var myChart = this.$echarts.init(document.getElementById("main"));
      //3. 指定图表的配置项和数据
      var option = {
        title: {
          text: "柱状图",
        },
        tooltip: {},
        legend: {
          //图例组件
          data: ["销量"],
        },
        xAxis: {
          data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"],
        },
        yAxis: {},
        series: [
          {
            name: "销量",
            type: "bar",
            data: [5, 20, 36, 10, 10, 20],
          },
        ],
      };
      //4.使用刚指定的配置项和数据显示图表。
      myChart.setOption(option);
    },
    drawLineChart() {
      //2. 基于准备好的dom，初始化echarts实例
      var myChart = this.$echarts.init(document.getElementById("main1"));
      //3. 指定图表的配置项和数据
      var option = {
        title: {
          text: "线性图",
        },
        tooltip: {},
        legend: {
          //图例组件
          data: ["日期"],
        },
        xAxis: {
          type: "category",
          data: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            data: [150, 230, 224, 218, 135, 147, 260],
            type: "line",
          },
        ],
      };
      //4.使用刚指定的配置项和数据显示图表。
      myChart.setOption(option);
    },
  },
  mounted() {
    this.drawBarChart();
    this.drawLineChart();
  },
};
</script>

<style scoped>
</style>
