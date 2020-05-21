<template>
  <div class="hello">
    <!-- 有宽高的盒子,用以初始化echarts-->
    <div ref="mapbox" style="height: 500px; width: 700px"></div>
  </div>
</template>

<script>
import echarts from "echarts";
import "echarts/map/js/china.js";

// 使用地图前需注册
const option = {
  title: {
    text: "疫情地图",
    link: "htpps://baidu.com",
    subtext: "加油!",
  },
  series: [
    {
      type: "map", // 要显示的类型
      map: "china", // 显示中国地图
      label: {
        show: true, // 控制省名的显示
        color: "#333",
        fontSize: 8,
      },
      itemStyle: {
        areaColor: "#eee", // 板块颜色
        borderColor: "#aaa", // 版跨边框颜色
      },
      zoom: 1.2,
      emphasis: {
        label: {
          show: true,
          color: "#f0f0f0",
          fontSize: 12,
        },
        itemStyle: {
          areaColor: "#f06",
        },
      },
    },
  ]
};

export default {
  name: "HelloWorld",
  mounted() {
    let mychart = echarts.init(this.$refs.mapbox);
    mychart.setOption(option);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
