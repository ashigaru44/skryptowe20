<template>
  <div id="app">
    
    <form>
      <div class="form-group row">
        <input type="date" class="form-control col-3 mx-2" name="Date from" v-model="date_from">
        <input type="date" class="form-control col-3 mx-2" name="Date to" v-model="date_to">
        <button type="submit" @click.stop.prevent="submit()">Show Rates</button>
      </div>
    </form>
    
    
    
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
  methods:{
    submit(){
      this.$router.push("http://localhost:8000/rates/"+this.date_from+'/'+this.date_to)
    }
  },
  async created(){
    var response = await fetch('http://localhost:8000/rates/');
    this.rates = await response.json();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
