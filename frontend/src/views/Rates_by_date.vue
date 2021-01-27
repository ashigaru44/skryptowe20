<template>
  <div class="rates col-10 mx-auto mt-5">    
    <form @submit.prevent>
      <div class="form-group row">
        <input type="date" class="form-control col-3 mx-2" name="Date from" v-model="date_from">
        <input type="date" class="form-control col-3 mx-2" name="Date to" v-model="date_to">
        <button type="submit" @click="get_rates_by_date()">Show Rates</button>
        <button class="float-right" type="button" @click="change_chart_mode()">Show Chart</button>
      </div>
    </form>

    <Chart :chartdata="this.data_collection" :options="options" chartLabel="label" v-if="this.rates.length > 0 && show_chart == true"/>

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
    <span class="font-weight-bold" v-if="this.rates.length == 0 && btn_pressed">Wrong dates entered, please enter correct dates! </span>
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
      btn_pressed: false,
      date_from: null,
      date_to: null,
      rates: [],
      data_collection: null,
      show_chart: false,
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
                  },
                  ticks: {
                    maxTicksLimit: 12,
                    autoSkip: true
                  }
              }],
              xAxes: [{
                  scaleLabel: {
                      display: true,
                  },
                  ticks: {
                    maxTicksLimit: 12,
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
    this.btn_pressed = true
    this.rates = await response.json()
    console.log(this.rates.length)
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
        label: "USD to PLN",
        data: rates,
        fill: false,
        pointBackgroundColor: "#FF0000",
        borderColor: "#B7B7B7"
      }]
    }
  },
  change_chart_mode() {
    if (this.show_chart) {
      this.show_chart = false
    }else{
      this.show_chart = true
    }
  }
}
}
</script>