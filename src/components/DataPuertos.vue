<template>
  <div>
    <div class="header">
      <img alt="img not found" width="170px" class="logo" src='../assets/cargofivalogo.png'/>
    <h1 class="title">Sea Ports</h1>
    <span class="span"><icon-search/><input v-model="searchQuery" type="text" placeholder="Search a port" class="searchbar" /></span> 
    </div>
      <div class="filtersDiv">
    <filter-functionality :filtertedByContinent="filtertedByContinent" class="filters"/>
    </div>
    <div class="contenedor">
    <table class="table">
      <thead>
      <tr>
        <br />
        <th>Name</th>
        <th>Country</th>
        <th>Continent</th>
        <th>coordinates</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in filteredPuertos" v-bind:key="item.id">
        <td>{{ item.id }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.country }}</td>
        <td>{{ item.continent }}</td>
        <td>{{ item.coordinates }}</td>
      </tr>
      </tbody>
    </table>
    </div>
    <div class="container-btn">
      <md-button class="md-primary md-raised" type="button" @click="page--"  v-if="page > 1">before</md-button>

      <md-button
        class="md-primary md-raised"
        type="button"
        :key="page"
      >
        {{page}}
      </md-button>
      <md-button class="md-primary md-raised" type="button"  @click="page++" v-if="page">Next</md-button>
    </div>
  </div>
</template>

<script>
import FilterFunctionality from './FiltersFunctionality.vue'
import IconSearch from '@/components/IconSearch.vue'
const {VUE_APP_API_URL} = process.env;

export default {
  name: "DataPuertos",
  components:{
    FilterFunctionality,
    IconSearch,
  },
  data() {
  
      return {
      list: [],
      page: 1,
      perPage:20,
      pages: [],
      searchQuery: "",
      data:[]
    };
  },

  mounted() {
    this.getData();
    console.log(process.env.VUE_APP_API_URL)
  },
  methods: {
    async getData() {
      for (let i = 1; i <= 25; i++) {
        const respuesta = await this.axios.get(
          `${VUE_APP_API_URL}${i}`
        );
        this.list.push(respuesta.data.data);
      }

      this.list = this.list.flat();
      this.data = this.list
      console.log(this.list);
    },
    paginate(list){
      let page = this.page;
      let perPage = this.perPage;
      let from = (page * perPage)-perPage;
      let to = (page * perPage)
      return list.slice(from, to)
    },
    filtertedByContinent(catName){
      this.list = this.data;
      if(catName !== 'All'){
        this.list = this.list.filter((el)=> {
          return el.continent === catName
        })
      }
    },
  },
   computed:{
       filteredPuertos() {
      const query = this.searchQuery.toLowerCase();
      if (this.searchQuery === "") {
        return this.paginate(this.list)
      }
        let puertosFiltrados = this.list.filter((el) => {
        return Object.values(el).some((word) => 
        String(word).toLowerCase().includes(query)
        );
      });
      return this.paginate(puertosFiltrados)
    },
    }
 
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.filtersDiv{
  display: flex;
   justify-content: center;
  align-content: center;
  background-color: #031b4e;
  min-width: fit-content;
}
.filters{
  display: flex;
  justify-content: center;
  align-content: center;
}
.md-button.md-raised:not([disabled]){
  color: #356eea;
}
.span{
  display: flex;
  justify-content: flex-end;
  margin: auto;
}
.searchbar{
  display: flex;
  justify-content: flex-end;
  align-content: center;
  width: 14rem;
  height: 30px;
  border: 1px solid #031b4e;
  border-radius: 1rem;
}
.header{
 display: flex;
 margin: 12px 0;
}
.title{
  font-family: Montserrat;
  color: #031b4e;
  font-weight: 300;
  display: flex;
  justify-content: center;
  margin: auto;
  font-size: 2.5rem;
}
img:not(.md-image){
  height: 40px;
  margin: auto;
  padding: auto;
}
.container-btn{
  display: flex;
  justify-content: center;
}
.md-button .md-ripple{
  background-color: #356eea;
  width: 2rem;
}
.contenedor{
  display: flex;
  justify-content: center;
  background-color: rgb(238,238,238);
  min-width: fit-content;
}
.table{
border-collapse: collapse;
margin: 30px 0;
font-size: 0.9em;
background: #ffffff;
width: auto;
border-radius: 5px 5px 0 0;
overflow: hidden;
}
.table td{
  margin: 5px 0;
  padding: 12px 15px;
  text-align: center;
  font-family: Montserrat;
  font-size: 15px;
  color: #031b4e;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.12);

}
.table thead tr {
  background-color: aliceblue;
  color:#031b4e;
  font-weight: bold;
}
.table tbody tr{
  border-bottom: 1px solid #dddddd;
}
.table tbody tr:nth-of-type(even){
  background-color: rgb(238,238,238);
}
.table tbody tr:last-of-type {
  border-bottom: 2px solid #356eea;
}
@media (max-width: 615px) {
  .searchbar{
    width: auto;
    height: auto;
    flex-direction: column;
  }
}
@media (max-width: 577px) {
  .title{
    font-size: 1.5rem;
  }
}
@media (max-width: 519px) {
  .searchbar{
    display: flex;
    flex-wrap: wrap;

  }
} 
@media (max-width: 468px) {
  .header{
    display: flex;
    flex-wrap: wrap;
  }
  .table{
    max-width: fit-content;
  }
  .contenedor{
    max-width: fit-content;
  }
  .table thead tr {
  max-width: fit-content;
}
}


@media (max-width: 410px) {
  .header{
    display: flex;
    flex-wrap: wrap;
    margin: auto;

  }
  .contenedor{
    max-width: fit-content;
  }
  .title{
    font-size: 32px;
  }
  .table{
    max-width: fit-content;
  }
}
@media (max-width: 280px) {
  .header{
    display: flex;
    flex-wrap: wrap;
    margin: auto;

  }
  .title{
    font-size: 32px;
  }
  .searchbar{
    margin: 2px 0;
  }
}

</style>
