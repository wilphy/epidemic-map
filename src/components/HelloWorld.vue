<template>
  <div class="hello">
    <!-- 有宽高的盒子,用以初始化echarts-->
    <div ref="mapbox" style="height: 500px; width: 700px"></div>
  </div>
</template>

<script>
import echarts from "echarts";
import "echarts/map/js/china.js";
import jsonp from "jsonp";

// 使用地图前需注册
const option = {
  title: {
    text: "疫情地图",
    link: "htpps://baidu.com",
    subtext: "中国加油!",
  },
  series: [
    {
      type: "map", // 要显示的类型
      map: "china", // 显示中国地图
      name: "累计确诊",
      label: {
        show: true, // 控制省名的显示
        color: "#333",
        fontSize: 8,
      },
      itemStyle: {
        areaColor: "#eee", // 板块颜色
        borderColor: "#aaa", // 版跨边框颜色
      },
      roam: true, // 鼠标缩放
      zoom: 1.2,
      emphasis: {
        label: {
          show: true,
          color: "#f0f0f0",
          fontSize: 12,
        },
        itemStyle: {
          areaColor: "#a3a1ef",
        },
      },
      data: [], // 展示后台数据
    },
  ],
  visualMap: {
    type: "piecewise",
    show: true,
    // splitNumber: 4,
    pieces: [
      // 分段
      {
        min: 10000,
      },
      {
        min: 1000,
        max: 9999,
      },
      {
        min: 100,
        max: 999,
      },
      {
        min: 10,
        max: 99,
      },
      {
        min: 1,
        max: 9,
      },
    ],
  },
  tooltip: {
    trigger: "item",
  },
  toolbox: {
    show: true,
    orient: "vertical",
    left: "right",
    top: "center",
    feature: {
      dataView: { readOnly: false },
      restore: "",
      saveAsImage: {},
    },
  },
};

export default {
  name: "HelloWorld",
  mounted() {
    this.getData();
    this.mychart = echarts.init(this.$refs.mapbox);
    this.mychart.setOption(option);
  },
  methods: {
    getData() {
      jsonp(
        "https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427&callback=__jp0",
        {},
        (err, data) => {
          if (!err) {
            // 请求数据成功
            console.log(data);
            let list = data.data.list.map((item) => ({
              name: item.name,
              value: item.value,
            }));
            console.log(list)
            option.series[0].data = list;
            this.mychart.setOption(option);
          }
        }
      );
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
