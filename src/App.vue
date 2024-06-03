<template>
  <div class="app">
    <filter-list @search="searchCountries"></filter-list>
    <loading-indicator :loading="loading"></loading-indicator>
    <grid-list ref="gridList" v-if="!loading"></grid-list>
  </div>
</template>

<script>
import FilterList from "./components/FilterList.vue";
import GridList from "./components/GridList.vue";
import LoadingIndicator from "./components/LoadingIndicator.vue";
import axios from "axios";

export default {
  components: {
    FilterList,
    GridList,
    LoadingIndicator,
  },
  data() {
    return {
      loading: false,
    };
  },
  methods: {
    async searchCountries({ query }) {
      console.log(query);
      let gridList;

      try {
        this.loading = true;
        gridList = this.$refs.gridList;
        let response;
        if (query) {
          response = await axios.get(
            `https://restcountries.com/v3.1/name/${query}`
          );
          console.log(JSON.parse(JSON.stringify(response)), "getting response");
          // } else if (region) {
          //   response = await axios.get(`https://restcountries.com/v3.1/region/${region}`);
          // }
        } else {
          response = await axios.get("https://restcountries.com/v3.1/all");
        }
        const countries = response.data;
        countries.sort((a, b) => b.population - a.population);
        if (gridList) {
          gridList.countries = countries.slice(0, 20).map((country) => ({
            name: country.name.common,
            capital: country.capital ? country.capital[0] : "N/A",
            currencies: country.currencies
              ? Object.values(country.currencies)
                  .map((c) => c.name)
                  .join(", ")
              : "N/A",
            region: country.region,
            latlng: country.latlng.join(", "),
            languages: country.languages
              ? Object.values(country.languages).join(", ")
              : "N/A",
            population: country.population,
            flag: country.flags.svg,
            details: country,
          }));
        }
      } catch (error) {
        console.error(error);
        if (gridList) {
          gridList.countries = [];
        }
      } finally {
        this.loading = false;
      }
    },
    // if(searchCountries){
    //   this.searchCountries(query);
    // }
  },
};
</script>

<style lang="scss">
.app {
  // background-color: #d1c7e0;
  max-width: 1200px;
  margin: 0 auto;
}
</style>
