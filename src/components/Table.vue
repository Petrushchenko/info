<template>
    <div>
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
                <td class="name" 
                    v-html="highlight(person.name, wordToHighlight)"
                ></td>

                <td class="email" v-html="highlight(person.email, wordToHighlight)">
                  <!--   <textarea @keyup.prevent.enter="changeData(person.id, person)"
                              v-model="person.email"
                           
                    >
                    {{person.email}}</textarea> -->
    
                </td>
                <td class="city" v-html="highlight(person.city, wordToHighlight)"></td>
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
      return keys.filter(key => key !== "id" && key !== "funds")
    }
  },
  methods: {
    highlight(text, word) {
        return text.replace(new RegExp(word, 'gi'), '<span class="hl">$&</span>');
    }
  },
  // watch: {
  //   wordToHighlight: {
  //       immediate: true,
  //       handler( val, oldVal) {
  //           console.log(val)
  //       }
  //   }
  // }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  table {
    width: 80%;
    margin: 0 auto;
    border-color: #A8698A;
    td {
      // textarea {
      //   box-sizing: border-box;
      //   resize: none;
      //   outline: none;
      //   width: 100%;
      //   height: 100%;
      //   border:none;
      //   &:hover {
      //       cursor: pointer;
      //     }
      // }
        cursor: pointer;
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
