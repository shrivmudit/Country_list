<template>
   <!-- <filter-list @search="searchCountries"></filter-list> -->
  <div class="grid-list-container">
    <table class="grid-list-table">
      <thead>
        <tr>
          <th>Flag</th>
          <th>Name</th>
          <th>Capital</th>
          <th>Currency</th>
          <th>Region</th>
          <th>Lat/Lng</th>
          <th>Language</th>
          <th>Population</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="country in countries" :key="country.name" @click="showDetails(country)" class="grid-list-row">
          <td><img :src="country.flag" class="grid-list-flag-img"></td>
          <td>{{ country.name }}</td>
          <td>{{ country.capital }}</td>
          <td>{{ country.currencies }}</td>
          <td>{{ country.region }}</td>
          <td>{{ country.latlng }}</td>
          <td>{{ country.languages }}</td>
          <td>{{ country.population }}</td>
        </tr>
      </tbody>
    </table>
    <grid-list-modal v-if="selectedCountry" :country="selectedCountry" @close="selectedCountry = null"></grid-list-modal>
  </div>
  
</template>

<script>
import axios from 'axios';
import GridListModal from './GridListModal.vue';
// import FilterList from './FilterList.vue'

export default {
  components: {
    GridListModal,
    // FilterList
  },
//   props: ['countries'],
  data() {
    return {
      countries: [],
      selectedCountry: null,
      query: '',
      // countries: [],
      filteredCountries: []
    };
  },
  methods: {
    async fetchCountries() {
        const forAllFetch = "all"
      try {
        const response = await axios.get(`https://restcountries.com/v3.1/${forAllFetch}`);
         this.countries = response.data;
        console.log(this.countries);
        this.countries.sort((a, b) => b.population - a.population);
        this.countries = this.countries.slice(0, 20).map(country => ({
          name: country.name.common,
          capital: country.capital ? country.capital[0] : 'N/A',
          currencies: country.currencies ? Object.values(country.currencies).map(c => c.name).join(', ') : 'N/A',
          region: country.region,
          latlng: country.latlng.join(', '),
          languages: country.languages ? Object.values(country.languages).join(', ') : 'N/A',
          population: country.population,
          flag: country.flags.svg,
          details: country,
        }));
         this.filteredCountries = this.countries;
      } catch (error) {
        console.error(error);
        this.countries = [];
      }
    },
    showDetails(country) {
      this.selectedCountry = country.details;
    },
  },
  created() {
    this.fetchCountries();
  },
  
  search() {
    console.log(this.countries,"Testing search");
      this.filteredCountries = this.countries.filter(country =>
        country.name.toLowerCase().includes(this.query.toLowerCase())
      )
      console.log(this.filteredCountries);
      }
    
};
</script>

<style scoped lang="scss">
.grid-list-container {
  max-width: 1000px;
  margin: 0 auto;
 background: #ceb9e5;
  // color: white;

}

.grid-list-table {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
}

.grid-list-table th,
.grid-list-table td {
  padding: 10px;
  text-align: left;
  border: 1px solid rgb(8, 36, 57);
}

.grid-list-row {
  cursor: pointer;
  &:hover {
    background-color: #f1f1f1;
  }
}

.grid-list-flag-img {
 
  width: 50px;
  height: auto;
}
</style>
