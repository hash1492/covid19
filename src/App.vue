<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <h1>Covid-19 Dashboard</h1>
    <div class="container">
      <b-form-input size="lg" class="search-input" placeholder="Search for a country" v-model="country"></b-form-input>
      <div class="countries-table">
        <b-table  sort-by="confirmed" :sort-desc="true" :filter="country" bordered striped hover :items="items" :fields="fields" :busy="isLoading">
          <template v-slot:cell(#)="data">
            {{ data.index + 1 }}
          </template>
          <template v-slot:table-busy>
            <div class="text-center text-danger my-2">
              <b-spinner class="align-middle"></b-spinner>
              <strong>Loading...</strong>
            </div>
          </template>
        </b-table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      isLoading: false,
      country: '',
      fields: [
        '#',
        {
          key: 'country',
          label: 'Country',
          sortable: true
        },
        {
          key: 'confirmed',
          label: 'Confirmed Cases',
          sortable: true
        },
        {
          key: 'recovered',
          label: 'Recovered Cases',
          sortable: true,
          variant: 'success'
        },
        {
          key: 'deaths',
          label: 'Total Deaths',
          sortable: true,
          variant: 'danger'
        }
      ],
      items: []
    }
  },
  created() {
    let self = this;
    self.isLoading = true;
    axios.get('https://corona-api.com/countries')
    .then(function (response) {
      const data = response.data.data;
      console.log(data);
      
      const countries = data.map(country => {
        return {
          country: country.name,
          confirmed: country.latest_data.confirmed,
          recovered: country.latest_data.recovered,
          deaths: country.latest_data.deaths
        }
      });
      self.items = countries;
      self.isLoading = false;
      console.log(self.items);
    })
    .catch(function (err) {
      console.log(err)
    })
  },
  // created() {
  //   let self = this;
  //   self.isLoading = true;
  //   axios.get('https://wuhan-coronavirus-api.laeyoung.endpoint.ainize.ai/jhu-edu/latest')
  //   .then(function (response) {
  //     const data = response.data;
  //     const countries = data.map(country => {
  //       if(country.countryregion === 'US') {
  //         country.countryregion = 'United States of America'
  //       }
  //       return {
  //         country: country.countryregion,
  //         province: country.provincestate,
  //         confirmed: country.confirmed,
  //         recovered: country.recovered,
  //         deaths: country.deaths,
  //       }
  //     });
  //     self.items = countries;
  //     self.isLoading = false;
  //     console.log(self.items);
  //   })
  //   .catch(function (err) {
  //     console.log(err)
  //   })
  // }
}
</script>
<style lang="scss">
@import 'node_modules/bootstrap/scss/bootstrap';
@import 'node_modules/bootstrap-vue/src/index.scss';
</style>

<style lang="scss">

.search-input {
  width: 300px;
  margin: 0 auto;
  margin-bottom: 30px;
}

strong {
  margin-left: 10px;
}
</style>

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
