<template>
  <div style="width: 100%; height: 400px;">
    <IEcharts :option="bar" :loading="loading"></IEcharts>
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
      loading: false,
      bar: {
        title: {
          text: 'ECharts bar + Ajax',
          x: 'center'
        },
        tooltip: {
          show: true
        },
        xAxis: {
          show: true,
          data: []
        },
        yAxis: {},
        series: [{
          type: 'bar',
          data: [
          ],
          barWidth: 20,
          barGap: '-100%'
        }],
        color: {
          type: 'linear',
          x: 0,
          y: 0,
          x2: 0,
          y2: 1,
          colorStops: [{
              offset: 0, color: '#CDDC39' // color at 0% position
          }, {
              offset: 1, color: '#8BC34A' // color at 100% position
          }],
          globalCoord: true // false by default
        }
      }
    }
  },
  mounted: function () {
    axios.get('/json/bantenprov/dd-peserta-didik/dd-peserta-didik-020.json').then(response => {

      let ke = 0;

      var res = response.data;

      /**
      * response :
      * console.log(res)
      *
      * xAxis
      * console.log(res[0].xAxis.data)
      * console.log(Object.values(res[0].xAxis.data))
      *
      * series data
      * console.log(res[0].series[0].data)
      *
      * region
      * console.log(res[0].xAxis.region)
      *
      * length
      * console.log(res.length);
      */

      this.bar.xAxis.data = Object.values(res[0].xAxis.data);
      this.bar.series[0].data = res[0].series[0].data;
      this.bar.title.text = res[0].xAxis.category + ' ' + res[0].xAxis.region + ' ' + res[0].xAxis.name;

      // interval
      let i = 0;

      setInterval(() => {

        this.bar.xAxis.data = Object.values(res[i].xAxis.data);
        this.bar.series[0].data = res[i].series[0].data;
        this.bar.title.text = res[0].xAxis.category + ' ' + res[i].xAxis.region + ' ' + res[i].xAxis.name;

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