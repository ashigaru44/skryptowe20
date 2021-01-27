<template>
  <div class="rates col-10 mx-auto mt-5">    
    <form @submit.prevent>
      <div class="form-group row">
        <input type="date" class="form-control col-3 mx-2" name="Date from" v-model="date_from">
        <input type="date" class="form-control col-3 mx-2" name="Date to" v-model="date_to">
        <button type="submit" @click="get_rates_by_date()">Show Rates</button>
      </div>
    </form>
    
    <Chart :chartdata="this.data_collection" :options="options" chartLabel="label" v-if="data_collection !== null"/>

    <table class="table">
      <thead>
        <th>Date</th>
        <th>Interpolated</th>
        <th>Rate</th>
        <th>Volume PLN</th>
        <th>Volume USD</th>
      </thead>
      <tbody>
        <tr v-for="rate in rates" :key="rate.date">
          <td>{{ rate.date }}</td>
          <td>{{ rate.rate }}</td>
          <td>{{ rate.interpolated }}</td>
          <td>{{ rate.volumePLN }}</td>
          <td>{{ rate.volumeUSD }}</td>
        </tr>
      </tbody>
    </table>  
  </div>
</template>

<script>
import Chart from "./Chart"

export default {
  // extends: Bar,
  // mounted () {
  //   this.renderChar(data, options)
  // },
  name: 'App',
  data(){
    return {
      
      date_from: null,
      date_to: null,
      rates: [],
      data_collection: null,
      // data = {
      //   labels: rates.
      // }
      options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
              yAxes: [{
                  scaleLabel: {
                      display: true,
                      labelString: ""
                  },
                  ticks: {
                    maxTicksLimit: 10,
                    autoSkip: true
                  }
              }],
              xAxes: [{
                  scaleLabel: {
                      display: true,
                      label: "",
                  },
                  ticks: {
                    maxTicksLimit: 10,
                    autoSkip: true
                  }
              }]
          }
        },
    } 
  },
  components: {
    Chart
  },
  methods:{
  async get_rates_by_date(){
    var response = await fetch('http://localhost:8000/rates/'+ this.date_from +'/'+ this.date_to)
    this.rates = await response.json()
    this.fill_data()
  },
  fill_data() {
    let rates = [];
    let dates = [];
    for (var i = 0; i < this.rates.length; i++) {
      rates.push(this.rates[i].rate)
      dates.push(this.rates[i].date)
    }
    this.options.scales.yAxes[0].scaleLabel.labelString = "Rate";
    this.options.scales.xAxes[0].scaleLabel.labelString = "Date";
    this.data_collection = {
      labels: dates,
      datasets: [{
        label: "USD Exchange rates",
        data: rates,
        fill: false,
      }]
    }
  }
}
}
</script>