<template>
<div class= "container-list">
  <form @submit.prevent="search" class="filter-list">
    <input v-model="query" class="filter-input" @input="searchItem" placeholder="Search by Country name">
    <button class="filter-button" type="submit">Search</button>
  </form>
</div>
</template>

<script>
export default {
    //  props: ['searchedItem'],
  data() {
    return {
      query: '',
      region: '',
      searchedItem: ''
    };
  },
  methods: {
    search() {
      this.$emit('search', { query: this.query, region: this.region });
        this.searchedItem = this.query;
    },

  }
};
</script>

<style scoped lang="scss">
.container-list {
display: flex;
flex-direction: column;

.filter-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 20px;
}

.filter-input,
.filter-select {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #EDE8F5;
  text-align: center;
  input:focus {outline:none;}
  box-shadow:none;
}

.filter-button {
  padding: 10px 20px;
  background: #997cb8;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  
}

.filter-button:hover {
  background: #997cb8;
}
}
</style>

