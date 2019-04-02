<template>
  <div id="app">
    <Search @searchWord="findMatches" 
            :searchError="searchError"
    />

    <Table :persons="shownPersons"
           :rowsPerPage="rowsPerPage"
           :currentPage="currentPage"
           :wordToHighlight="wordToHighlight"
           :if="shownPersons"
           
    />
<!-- @changeData="setData" -->
    <Pagination :persons="filteredPersons.length > 0 ? filteredPersons : persons"
                :currentPage="currentPage"
                :rowsPerPage="rowsPerPage"
                @pageChanged="changePage"

    />
  </div>
</template>

<script>
import axios from 'axios';
import Table from './components/Table.vue'
import Pagination from './components/Pagination.vue'
import Search from './components/Search.vue'

const API_URL = 'http://localhost:8080';

export default {
  name: 'app',
  components: {
    Table,
    Pagination,
    Search
  },
  data(){
    return {
      persons: [],
      rowsPerPage: 10,
      currentPage: 1,
      wordToHighlight: "",
      filteredPersons: [],
      searchError: false
    }
  },
  methods: {
    getData(){
      let url = `${API_URL}/?funds:200,city:`;
      return axios.get(url)
      .then((response) => {
        this.persons = response.data.data
        .map(person => {
          for (let key in person) {
            person[key] = key === "funds" ? parseInt(person[key]) : person[key]
          }
          return person;
        })

        let reg =  /(?<=funds:)-*\d+/;

        if (reg.test(url)) {
          this.persons = this.persons.filter( person => (+url.match(reg)[0] > 0 )? person.funds > +url.match(reg)[0] : person.funds < Math.abs(+url.match(reg)[0]))
          } 
      })
      .catch(function (error) {
        console.log(error);
      });
    },
    changePage(num){
      this.currentPage = num
    },
    // setData(id, data){
    //   console.log(data)
    //   let url = `${API_URL}/update/:id`;
    //   return axios.put(url, data)
    //     .then(function (response) {
    //       console.log(response.data);
    //     })
    //     .catch(function (error) {
    //         console.log(error);
    //         alert('Sorry! Server is not able to process this request at the moment.')
    //     });
    // },
    findMatches(wordToMatch) {
      this.wordToHighlight = wordToMatch
      const regex = new RegExp(wordToMatch, 'gi');
      this.filteredPersons = this.persons.filter(person => person.name.match(regex) || person.city.match(regex) || person.email.match(regex));
      this.filteredPersons.length === 0 ? this.searchError = true : this.searchError = false;
      if (this.filteredPersons.length >0 && this.filteredPersons.length <= this.rowsPerPage) {
        this.currentPage = 1
      }
    },
  },
  computed: {
    shownPersons(){

      let startIndex = (this.currentPage - 1) * this.rowsPerPage;
      return this.filteredPersons.length > 0 ? this.filteredPersons.slice(startIndex, startIndex + this.rowsPerPage) : this.persons.slice(startIndex, startIndex + this.rowsPerPage)
    }
  },
  created() {
    this.getData();
  }
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
  .hl {
    background-color:#ffc600;
  }
}
</style>
