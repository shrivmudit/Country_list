<template>
  <div class="grid-list-modal-backdrop">
    <div class="grid-list-modal">
      <h2 class="grid-list-modal-title">{{ country.name.official }}</h2>
      <p><strong>Official Name:</strong> {{ country.name.official }}</p>
      <p><strong>Sub Region:</strong> {{ country.subregion }}</p>
      <p><strong>UN Member:</strong> {{ country.unMember ? 'Yes' : 'No' }}</p>
      <p><strong>Border Countries:</strong> {{ borderCountries }}</p>
      <button class="grid-list-modal-close" @click="close">Close</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    country: Object,
  },
  computed: {
    borderCountries() {
      return this.country.borders ? this.country.borders.join(', ') : 'None';
    },
  },
  methods: {
    close() {
      this.$emit('close');
    },
  },
};
</script>

<style scoped lang="scss">
.grid-list-modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.grid-list-modal {
  background-color: #EDE8F5;
  padding: 20px;
  border-radius: 10px;
  max-width: 500px;
  width: 100%;
  color: rgb(63, 12, 86);
}

.grid-list-modal-title {
  margin-bottom: 10px;
}

.grid-list-modal-close {
  // margin-top: 20px;
  padding: 10px 20px;
  background: #997cb8;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.grid-list-modal-close:hover {
   background:rgb(155, 122, 193)8;
}
</style>
