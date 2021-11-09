<script>
	import RandomVm from "./RandomVm.svelte"
</script>

<main>
	<div class="main-section">
		<RandomVm />
	</div>
	<footer><a href="https://github.com/bmlt-enabled/randomvm/issues" target="_blank">Ideas?</a> | Meetings are sourced from <a href="https://virtual-na.org/meetings" target="_blank">https://virtual-na.org/meetings</a></footer>
</main>

<style>
	main {
		width: 95vw;
	}

	.main-section {
		display: flex;
		flex-wrap: wrap;
		align-content: center;
		justify-content: center;
	}
	p{
		text-align: center;
	}
	footer {
		text-align: center;
	}
</style>