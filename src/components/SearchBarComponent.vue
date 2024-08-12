<template>
  <div class="search-div">
    <input class="search-input" type="text" v-model="searchQuery" placeholder="Поиск" @change="queryUpdate">
  </div>
</template>

<script>
export default {
  name: "SearchBarComponent",
  emits: [
    'send_search_query',
  ],
  data() {
    return {
      searchQuery: '',
    }
  },
  methods: {
    queryUpdate(){
      this.$emit('send_search_query', this.searchQuery);
    }
  },
}
</script>

<style scoped>

</style>