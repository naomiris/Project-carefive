<template>
<v-app>
  <div id="app">
  <DataPuertos/>
  </div>
  </v-app>
</template>

<script>
import DataPuertos from './components/DataPuertos.vue';



export default {
  name: 'App',
  components: {
    DataPuertos,
  },
}
</script>

<style>
</style>
