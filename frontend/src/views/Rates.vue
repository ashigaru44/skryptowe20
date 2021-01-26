<template>
  <div class="rates col-8 mx-auto mt-5">
    <table class="table">
      <thead>
        <th>Date</th>
        <th>Interpolated</th>
        <th>Rate</th>
      </thead>
      <tbody>
        <tr v-for="rate in rates" :key="rate.date">
          <td>{{ rate.date }}</td>
          <td>{{ rate.rate }}</td>
          <td>{{ rate.interpolated }}</td>
        </tr>
      </tbody>
    </table>  
  </div>
</template>

<script>
export default {
  name: 'App',
  data(){
    return {
      date_from: null,
      date_to: null,
      rates: []
    } 
  },
  async created(){
    var response = await fetch('http://localhost:8000/rates/');
    this.rates = await response.json();
  }
}
</script>