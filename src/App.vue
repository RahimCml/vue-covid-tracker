<script>

import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      countries: [],
      selectedCountry: '',
      data: 0,
      countryData: {},
      confirmed: null,
      deaths: null,
      recovered: null
    }
  },
  methods: {
    getCountries () {
      return axios.get('https://api.covid19api.com/countries')
        .then(response => {
          this.countries = response.data
        })
    }
  }
}
</script>

<template>
  <div class="container">
  </div>
</template>

<style>
  body {
      background-image: url("https://wallpaperaccess.com/full/1548134.jpg");
  }
</style>
