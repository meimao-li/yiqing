<template>
  <div class="hello">
    <div ref="mapbox" style="width: 600px ; height: 400px" class="tu"></div>
  </div>
</template>

<script>

  import echarts from 'echarts'
  import 'echarts/map/js/china.js'
  import jsonp from 'jsonp'



const option = {
    title: {
      text: "国内疫情地图",
      link : "https://www.sina.com.cn/",
      subtext : "来自于新浪",
      sublink : "https://www.sina.com.cn/"

    },
    series : [{
      name: "确诊人数",
      type : "map", //告诉echarts要去渲染的是一个地图
      map : "china", //告诉echarts要去渲染中国地图
      label :{
        //控制对应地区的汉字
        show :true,
        color : "red",//控制地区名的字体颜色
        fontSize : 11 //控制地区名的字体大小
      },
      itemStyle : {
        areaColor : "#53868B",//地区模块的颜色
        borderColor : "#98FB98", //地区模块的分界线的颜色
        borderType : "dashed", //地区模块的分界线
        opacity :0.7, ////地区模块的透明度 0-1
      },
      roam : true,
      zoom : 1.2,//控制地图放大缩小
      emphasis : {  //控制鼠标滑过之后的背景颜色和字体颜色
        label: {
          color: "#7FFFD4",
          fontSize: 12
        },
        itemStyle: {
          areaColor:"#3A5FCD"
        }
      },
      //用来展示后台给的数据
      data : []
    }],
    visualMap:[{
      type :"piecewise", //分条
      show: true, //显示
      // splitNumber : 4 //出现的条数
      pieces :[
        //分段
        {min : 10000},
        {min: 1000 , max: 9999},
        {min: 100 , max: 999},
        {min: 10 , max: 99},
        {min: 1 , max: 9}
      ],
      // align :"right", //默认是left
      // orient : "horizontal"
      //left right 这些属性控制 分段坐在的位置
      // showLabel:false 数据的显示或隐藏
      // textStyle:{}
      inRange : {
        symbol : "rect",//circle 和 rect
        color : ["blue","red"]
      },
      itemWidth : 20,
      itemHeight : 10
    }],
    tooltip:{
      tooltip :"item"
    },
      toolbox: {
      show: true,
      orient: 'vertical',
      left: 'right',
      top: 'center',
      feature: {
      dataView: {readOnly: false},
      restore: {},
      saveAsImage: {}
    }
  }


}
export default {
  name: 'HelloWorld',
  mounted() {
    this.getData();
    this. myChart =  echarts.init(this.$refs.mapbox);
    this.myChart.setOption(option);
  },
  methods : {
    getData(){
      jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',{},(err,data) =>{
        if (!err){
          //代表请求数据成功
          console.log(data);
          let list = data.data.list.map(item=>({name:item.name,value:item.value}));
          option.series[0].data = list;
          this.myChart.setOption(option);
        }
      })
    }
  }

}
</script>


<style scoped>

  .tu{
    margin: 0 auto;
  }
</style>
