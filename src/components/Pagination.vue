<template>
	<div class="pagination">
		<div class="prev">Previous</div>
		<div class="next">Next</div>
	</div>

</template>

<script>
	export default {

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
			&:hover {
				cursor: pointer;
				border-color: #A8698A;
				box-shadow: 0 0 5px black;
			}
		}
	}
</style>