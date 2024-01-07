<script>
	import Card from "../lib/components/Card.svelte";
	import Table from "../lib/components/Table.svelte";
	
</script>

<main>
	
	<Card>
		<Table />
	</Card>
</main>

<style>
	main {
		display: grid;
		grid-template-columns: 100px;
		grid-template-rows: 100px 100px;
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0px auto;
		margin-top: 100px;
	}
</style>
