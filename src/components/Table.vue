<template>
    <div>
        <table cellspacing="0" rules="cols rows" border="1" :if="persons">
            <thead>
              <tr>
                <td v-for="(field, index) in tableFields"
                    :key="index"
                >
                  {{ field }}                  
                </td>
              </tr>
            </thead>
            <tbody>
              <tr v-for="person in persons">
                <td>{{ person.id }}</td>
                <td class="name">
                  <div v-if="!person.editable" 
                       @click="makeEditable( person )"
                       v-html="highlight(person.name, wordToHighlight)"
                  >    
                  </div>
                  <textarea v-else 
                            @keypress.enter.prevent="changeData(person)"
                            v-model="person.name"
                            autofocus
                            @mouseout="makeUneditable(person)"
                    >
                    {{person.name}}</textarea>
                    
                </td>

                <td class="email" v-html="highlight(person.email, wordToHighlight)">
     
    
                </td>
                <td class="city" v-html="highlight(person.city, wordToHighlight)"></td>
                <td class="phone">{{person.phone}}</textarea></td>
                <td>{{ person.editable }}</td>
              </tr>
            </tbody>
        </table> 
        
    </div>
</template>

<script>
export default {
  name: 'Table',
  props: {
    persons: Array,
    rowsPerPage: Number,
    currentPage: Number,
    wordToHighlight: String
  },
  computed: {
    tableFields() {
      let keys = Object.keys(this.persons[0])
      return keys.filter(key => key !== "funds") //key !== "id" && 
    }
  },
  methods: {
    highlight(text, word) {

        return word.length > 0 ? text.replace(new RegExp(word, 'gi'), '<span class="hl">$&</span>') : text;
    },
    makeEditable(person) {
      this.persons.forEach( item => item.id === person.id ? item.editable = true : null);
    },
    makeUneditable(person) {
       this.persons.forEach( item => item.id === person.id ? item.editable = false : null);
      
    },
    changeData (person) {
      this.persons.forEach( item => item.id === person.id ? item.editable = false : null);
      this.$emit("changeData", person)
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
      cursor: pointer;
      textarea {
        box-sizing: border-box;
        resize: none;
        outline: none;
        width: 100%;
        height: 100%;
        border:none;
      }
      div {
        padding: 10px 0;
      }
    }
  }
  .popup {
    position: absolute;
    padding: 10px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 200px;
    background-color: #ae7272;
  }
</style>
