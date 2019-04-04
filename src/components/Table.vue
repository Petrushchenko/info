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
                <td class="name">
                  <div v-if="!person.editable" 
                       @click="makeEditable( person )"
                       v-html="highlight(person.name, wordToHighlight)"
                  ></div>
                  <textarea v-else 
                            @keypress.enter.prevent="changeData(person)"
                            v-model="person.name"
                            autofocus
                    ></textarea> 
                </td>

                <td class="email">
                  <div v-if="!person.editable" 
                       @click="makeEditable( person )"
                       v-html="highlight(person.email, wordToHighlight)"
                  ></div>
                  <textarea v-else 
                            @keypress.enter.prevent="changeData(person)"
                            v-model="person.email"
                            autofocus
                    ></textarea>
                </td>
                <td class="city">
                  <div v-if="!person.editable" 
                       @click="makeEditable( person )"
                       v-html="highlight(person.city, wordToHighlight)"
                  ></div>
                  <textarea v-else 
                            @keypress.enter.prevent="changeData(person)"
                            v-model="person.city"
                            autofocus
                    ></textarea>
                </td>
                <td class="phone">{{person.phone}}</textarea></td>
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
      return keys.filter(key => key !== "id" && key !== "funds" && key !=="editable")
    }
  },
  methods: {
    highlight(text, word) {

        return word.length > 0 ? text.replace(new RegExp(word, 'gi'), '<span class="hl">$&</span>') : text;
    },
    makeEditable(person) {
      this.persons.forEach( item => item.id === person.id ? item.editable = true : null);
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
    tr:hover {
      background-color: #f2f2f2;
    }
    td {
      cursor: pointer;
      textarea {
        box-sizing: border-box;
        resize: none;
        outline: none;
        width: 100%;
        height: 100%;
        border:none;
        background-color: #f2f2f2;

      }
      div {
        padding: 10px 0;
      }
    }
  }
</style>
