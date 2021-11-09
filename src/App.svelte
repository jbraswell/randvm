<script>
	import RandomVm from "./RandomVm.svelte"

	export let name;

	function convert(){

	}

</script>

<main>
	<nav class="nav">
	<a href="/" class="app-title">{name}</a>
	</nav>
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
	.clock-section {
		display: flex;
		flex-wrap: wrap;
		align-content: center;
		justify-content: center;
	}
	.nav {
		padding: 20px;
	}
	p{
		text-align: center;
	}
	footer {
		text-align: center;
	}

	.app-title {
		color: #2b283b;
		text-transform: uppercase;
		font-size: 2rem;
		font-weight: 700;
		padding: 1vh 1vw;
		border: 8px solid #2b283b;
	}		
</style>