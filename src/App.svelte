<script>
	import RandomVm from "./RandomVm.svelte"
</script>

<main>
	<div class="main-section">
		<RandomVm />
	</div>
	<footer><a href="https://github.com/bmlt-enabled/randomvm/issues">Ideas?</a> | Meetings are sourced from https://virtual-na.org/meetings</footer>
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