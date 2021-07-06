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
    },
    getSummeryData () {
      return axios.get('https://api.covid19api.com/summary')
        .then(response => {
          const data = response.data
          this.data = data.Countries
          this.confirmed = data.Global.TotalConfirmed
          this.deaths = data.Global.TotalDeaths
          this.recovered = data.Global.TotalRecovered
        })
    },
    getDataByCountry () {
      var country = this.data.filter(country => {
        return country.Slug === this.selectedCountry
      })
      country = country[0]

      axios.get('https://api.covid19api.com/dayone/country/' + this.selectedCountry)
        .then(response => {
          this.countryData = response.data
          this.confirmed = country.TotalConfirmed
          this.deaths = country.TotalDeaths
          this.recovered = country.TotalRecovered
        })
    }
  },
  mounted () {
    this.getCountries()
    this.getSummeryData()
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
