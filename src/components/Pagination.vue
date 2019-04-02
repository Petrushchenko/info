<template>
	<div class="pagination">
		<div class="prev" 
             v-if="hasPrev"
             @click="changePage(currentPage - 1)">Previous</div>
             <span>{{currentPage}} from {{totalPages}}</span>
		<div class="next"
             v-if="hasNext"
             @click="changePage(currentPage + 1)"
        >Next</div>
	</div>

</template>

<script>
	export default {
        props: {
            persons: Array,
            currentPage: Number,
            rowsPerPage: Number
        },
        data() {
            return {

            }
        },
        computed: {
            totalPages() {
                return Math.ceil(this.persons.length / this.rowsPerPage)
            },
            hasPrev() {
              return this.currentPage > 1
            },
            hasNext() {
              return this.currentPage < this.totalPages
            },
        },
        methods: {
            changePage(pageNum) {
                this.$emit('pageChanged', pageNum)
            }
        }
	}
</script>

<style scoped lang="scss">
	.pagination {
		margin: 20px auto 0;
		width: 80%;
		display: flex;
		justify-content: space-between;
		.prev, .next {
			padding: 5px;
			font-weight: bold;
			border: 1px solid green;
			border-radius: 3px;
            width: fit-content;
			&:hover {
				cursor: pointer;
				border-color: #A8698A;
				box-shadow: 0 0 5px black;
			}
		}
        span {
            align-self: center;
        }
	}
</style>