<script>
import axios from 'axios'
//import * as $ from 'jquery';
export default {
  data() {
    return {
      chart: null,
      options: {
        animationEnabled: true,
        exportEnabled: true,
        title: {
          text: "Last 20 US Dollar exchange rates"
        },
        axisY: {
          title: "Exchange Rate",
          labelFormatter: (e) => {
            return e.value.toFixed(4);
          }
        },
        data: [{
  type: "line",
  xValueFormatString: "YYYY-MM-DD",
  dataPoints: []
}]
      },
      styleOptions: {
        width: "100%",
        height: "360px"
      }
    }
  },
  methods: {
  parseDataAndRenderChart(url) {
    axios.get(url)
      .then(response => {
        console.log('API Response:', response.data);

        for (var i = 0; i < response.data.rates.length; i++) {
          const dataPoint = {
            x: new Date(response.data.rates[i]["effectiveDate"]),
            y: response.data.rates[i]["mid"]
          };

          console.log('Adding Data Point:', dataPoint);

          this.options.data[0].dataPoints.push(dataPoint);
        }

        console.log('Final Chart Options:', this.options);
        this.chart.render();
      })
      .catch(error => {
        console.error('Error fetching data:', error);
      });
  },
    chartInstance(chart) {
      this.chart = chart;
      this.parseDataAndRenderChart("https://api.nbp.pl/api/exchangerates/rates/a/usd/last/20/?format=json");
    }
  }
}
</script>

<template>
  <CanvasJSChart :options="options" :style="styleOptions" @chart-ref="chartInstance"/>
</template>
