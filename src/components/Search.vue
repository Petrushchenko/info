<template>
    <form>
        <input type="search" 
               name="search"
               v-model="wordToSearch"
               @input="search(wordToSearch)"
               >

        <p v-if="searchError">Nothing found</p>
    </form>
</template>

<script>
    export default {
        props:{
            searchError: Boolean
        },
        data(){
            return {
                wordToSearch: ''
            }
        },
        methods: {
            search(word) {
                this.$emit("searchWord", word)
            },
        }
    }
</script>

<style scoped lang="scss">
    form {
        width: 80%;
        margin: 0 auto 30px;
        p {
            display: block;
            border: 1px solid red;
        }
    }
</style>