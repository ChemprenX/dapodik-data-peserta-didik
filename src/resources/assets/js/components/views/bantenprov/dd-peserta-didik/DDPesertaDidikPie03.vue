<template>
  <div style="width: 100%; height: 200px;">
    <IEcharts :option="pie"></IEcharts>
  </div>
</template>

<script>
import IEcharts from 'vue-echarts-v3/src/full.js';

export default {
  components: {
    IEcharts
  },
  data () {
    return {
      pie: {
        backgroundColor: '#28A745',
        title: {
          text: 'ECharts pie',
          left: 'center',
          top: 0,
          textStyle: {
            color: '#000',
            fontSize: 11
          }
        },
        tooltip: {
          show: true,
          trigger: 'item',
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        visualMap: {
          show: false,
          min: 100,
          max: 2000,
          inRange: {
            colorLightness: [0, 1]
          }
        },
        series : [
          {
            type:'pie',
            radius : '55%',
            center: ['50%', '50%'],
            data:[].sort(function (a, b) { return a.value - b.value; }),
            roseType: 'radius',
            label: {
              normal: {
                textStyle: {
                  color: '#444'
                }
              }
            },
            labelLine: {
              normal: {
                lineStyle: {
                  color: '#444'
                },
                smooth: 0.2,
                length: 10,
                length2: 5
              }
            },
            color: {
             roseType: 'radius',
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [{
                  offset: 0, color: '#CDDC39' // color at 0% position
              }, {
                  offset: 1, color: '#2893BD' // color at 100% position
              }],
              globalCoord: false // false by default
            },

            animationType: 'scale',
            animationEasing: 'elasticOut',
            animationDelay: function (idx) {
              return Math.random() * 200;
            }
          }
        ]
      }
    }
  },
  mounted: function () {
    axios.get('/json/bantenprov/dd-peserta-didik/dd-peserta-didik-pie-030.json').then(response => {

      let ke = 0;

      var res = response.data;

      this.pie.series[0].data = res[0].series[0].data;
      this.pie.title.text = res[0].xAxis.region + ' ' + res[0].xAxis.name;

      // interval
      let i = 0;

      setInterval(() => {

        this.pie.series[0].data = res[i].series[0].data;
        this.pie.title.text = res[i].xAxis.region + ' ' + res[i].xAxis.name;

        i++;

        if(i == res.length)
        {
          i = 0;
        }

      },4000);

    })
    .catch(function(error) {
      // error
    });
  }
}
</script>