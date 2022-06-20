<script>
  import Board from './Board.svelte';
</script>

<main>
	<h1>2048</h1>
	<p>Use your keyboard direction arrows to move the board!</p>

	<Board />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;

		margin: 0 auto;
	}

	h1 {
		color: rgb(3, 9, 78);
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 800;
	}

	@media (min-width: 640px) {
		main {
		}
	}
</style>