<template>
  <div>
    <input v-model="searchTerm" placeholder="Поиск по имени или параметрам" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: "",
    };
  },
  watch: {
    searchTerm: function (newSearchTerm) {
      this.$emit("filterChanged", newSearchTerm);
    },
  },
};
</script>
