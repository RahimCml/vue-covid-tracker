<script>

import axios from 'axios'
import Countries from './components/Countries.vue'
import Country from './components/Country.vue'

export default {
  name: 'App',
  components: {
    Country,
    Countries

  },
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
    <div class="form-group">
        <select v-model="selectedCountry" class="form-control" v-if="countries" @change="getDataByCountry">
          <option value="">Global</option>
          <option v-for="country in countries" :value="country.Slug" :key="country.Slug">{{country.Country}}
          </option>
        </select>
    </div>
    <div v-if="data">
      <div class="row justify-content-around">
        <div class="col-md-4">
          <div class="stats-item btn-info">
            <span class="item-title">Sağalma</span>
            <span class="item-count">{{confirmed | numberFormat}}</span>
          </div>
        </div>
        <div class="col-md-4">
          <div class="stats-item btn-danger">
            <span class="item-title">Ölüm halları</span>
            <span class="item-count">{{deaths | numberFormat}}</span>
          </div>
        </div>
        <div class="col-md-4">
          <div class="stats-item btn-success">
            <span class="item-title">Yoluxma</span>
            <span class="item-count">{{recovered | numberFormat}}</span>
          </div>
        </div>
      </div>
      <hr>
      <component :is="tableType" :data="tableData"></component>
    </div>
  </div>
</template>

<style>
  body {
      background-image: url("https://wallpaperaccess.com/full/1548134.jpg");
  }
  table {
    background: #fff;
  }
  .container {
    margin-top: 40px;
  }
  .stats-item {
    padding: 20px;
    text-align: center;
    margin-bottom: 20px;
  }
  .btn-danger {
    background-image: url(https://wgntv.com/wp-content/uploads/sites/5/2021/02/GettyImages-1292617439-1.jpg?w=2560&h=1440&crop=1);
    background-position: 54% 51%;
    background-size: 359px 184px;
  }
  .btn-success {
    background-image: url(https://ihpi.umich.edu/sites/default/files/styles/hero_slide/public/2020-03/GettyImages-1202969008-yellow_0.jpg?itok=YhuJvnYE);
    background-position: 132% 42%;
    background-size: 350px 153px;
  }
  .btn-info {
    background-image: url(https://www.biofin.org/sites/default/files/content/news_media/cvd_1.jpg);
    background-position: -2px;
  }
  .item-title {
    display: block;
    color: #fff;
    font-size: 17px;
  }
  .item-count {
    font-weight: bold;
    font-size: 30px;
  }
</style>
