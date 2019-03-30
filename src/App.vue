<template>
  <div id="app">
    
    <Table :persons="shownPersons"
           :rowsPerPage="rowsPerPage"
           :currentPage="currentPage"
           :if="shownPersons"
    />

    <Pagination/>
  <!-- {{shownPersons}} -->
  </div>
</template>

<script>
import axios from 'axios';
import Table from './components/Table.vue'
import Pagination from './components/Pagination.vue'


const API_URL = 'http://localhost:8080';

export default {
  name: 'app',
  components: {
    Table,
    Pagination
  },
  data(){
    return {
      persons: [],
      rowsPerPage: 10,
      currentPage: 1,
      count: 0
    }
  },
  methods: {
    getData(){
      return axios.get(API_URL)
      .then(response => response.data)
      .then((data) => {

          this.persons = data.data;
          console.log(this.persons);
          this.count = this.persons.length;
      })
      .catch(function (error) {
        console.log(error);
      });
    },
  },
  computed: {
    shownPersons(){
      let startIndex = (this.currentPage - 1) * this.rowsPerPage;
      return this.persons.slice(startIndex, startIndex + this.rowsPerPage);
    }
  },
  created() {
    this.getData();
    },
  }
</script>

<style lang="scss">

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
