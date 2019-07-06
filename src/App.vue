<template>
  <div id="app">
    <graph :chart-data="datacollection" :options="options" ref="graph"></graph>
  </div>
</template>

<script>
import Vue from 'vue'
import Graph from './components/Graph.vue'
import VueJsonp from 'vue-jsonp'
Vue.use(VueJsonp)

export default {
  name: 'app',
  components: {
    Graph
  },
  data (){
    return {
      datacollection: {
        type: "line",
        labels: [],
        datasets: [
            {
              label: 'Weight',
              data: []
            }
          ]
        },
      options: {
        title: {
          display: true,
          fontSize: 30,
          text: 'MAEKAWA \'s WEIGHT - July -'
        },
        scales: {
          yAxes: [{
            display: true,
            scaleLabel: {
              display: true,
              labelString: 'Weight (kg)',
              fontSize: 24
            },
          ticks: {
            min: 75,
            max: 85,
            fontSize: 9,
            stepSize: 0.5,
          }
          }],
          xAxes: [{
            display: true,
            scaleLabel: {
              display: true,
              labelString: 'day',
              fontSize: 10
            },
          ticks: {
            min: 1,
            max: 31,
            fontSize: 18,
            stepSize: 1,
          }
          }]
        }
      },
      jsonData: '',
    }
  },
  mounted (){
    this.getData("https://script.google.com/macros/s/AKfycbyNKgWu50IpEGkXCn4YUYUM42cridVKLpFynP6s7LNLmYSJM50/exec")
  },
  methods: {
    setDataSet(){
      
      for (var i = 0; i < this.jsonData.length; i++){
        //this.datacollection.labels.push(item["time"])
        this.datacollection.labels.push(i+1)
        this.datacollection.datasets[0].data.push(this.jsonData[i]["weight"])
      }
        // eslint-disable-next-line
        console.log(this.datacollection)
    },
    getData(apiUrl) {
      this.$jsonp(apiUrl, {'callback': 'result'}).then(json => {
        this.jsonData = json
        this.setDataSet()
        this.$refs.graph.render()
      }).catch(err => {
        // eslint-disable-next-line
        console.log(err)
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
