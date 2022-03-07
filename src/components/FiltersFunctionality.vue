<template>
  <div>
    <button
      v-for="filter in filters"
      :key="filter"
      class="buttonFilters"
      @click="() => filtertedByContinent(filter)"
    >
      {{ filter }}
    </button>
  </div>
</template>

<script>
const filters = ["All", "AS", "EU", "Asia", "AF", "NA", "OC", "SA"];

export default {
  name: "FiltersFunctionality",
  props: ["filtertedByContinent"],
  data() {
    return {
      filters,
    };
  },
};
</script>

<style scoped>
.buttonFilters{
 display: flex;
 margin: 3px 4px;
 border: 1px solid #ffff;
 background-color: transparent;
 color: #ffff;
 border-radius: 10px;
 width: auto;
 cursor: pointer;
 padding: 5px 10px;
}
.buttonFilters:hover{
  background-color:#356eea ;
}

</style>