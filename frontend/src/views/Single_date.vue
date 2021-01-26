<template>
  <div class="single_rate col-10 mx-auto mt-5">
    <form @submit.prevent>
      <div class="form-group row">
        <input type="date" class="form-control col-3 mx-2" name="Date" v-model="day_date">
        <button type="submit" @click="get_rates_by_date()">Show Day Rate</button>
      </div>
    </form>
    
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
// import { Bar } from 'vue-chartjs'

export default {
  // extends: Bar,
  // mounted () {
  //   this.renderChar(data, options)
  // },
  name: 'App',
  data(){
    return {
      day_date: null, 
      rates: [],
      // data = {
      //   labels: rates.
      // }
    } 
  },
  methods:{
  async get_rates_by_date(){
    var response = await fetch('http://localhost:8000/volume/'+ this.day_date);
    this.rates = await response.json();
  }
  }
}
</script>