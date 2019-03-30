<template>
  <table cellspacing="0" rules="cols rows" border="1" :if="persons">
    <thead>
      <tr>
        <td v-for="(field, index) in tableFields"
            :key="index"
        >
          {{field}}
        </td>
      </tr>
    </thead>
    <tbody>
      <tr v-for="person in persons">
        <td class="name"><textarea>{{person.name}}</textarea></td>
        <td class="email">{{person.email}}</td>
        <td>{{person.founds}}</td>
        <td>{{person.city}}</td>
        <td>{{person.phone}}</td>
      </tr>
    </tbody>
  </table> 
</template>

<script>
export default {
  name: 'Table',
  props: {
    persons: Array,
    rowsPerPage: Number,
    currentPage: Number
  },
  computed: {
    tableFields() {
      let keys = Object.keys(this.persons[0])
      return keys.filter(key => key !== "id")
    }
  },


}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  table {
    width: 80%;
    margin: 0 auto;
    border-color: #A8698A;
    td {
      padding: 2px;
    }
  }
</style>
