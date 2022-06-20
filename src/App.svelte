<script>
  import Board from './Board.svelte';
</script>

<main>
	<h1>2048</h1>
	<p>Use your keyboard's directional arrows to move the board!</p>

	<Board />
</main>

<style>
	:root {
  	--light-bg: white;
  	--light-mg: rgb(3, 9, 78);
		--light-text: rgb(3, 9, 78);;
		--light-cell: rgb(196, 198, 212);

  	--dark-bg: rgb(3, 9, 78);
  	--dark-mg: rgba(196, 198, 212, 0.3);
		--dark-text: white;
		--dark-cell: rgb(196, 198, 212);

		--bg: var(--light-bg);
		--mg: var(--light-mg);
		--text: var(--light-text);
		--cell: var(--light-cell);
	}

	@media (prefers-color-scheme: dark) {
		:root{
			--bg: var(--dark-bg);
			--mg: var(--dark-mg);
			--text: var(--dark-text);
			--cell: var(--dark-cell);
		}
	}

	@media (prefers-color-scheme: light) {
		:root{
			--bg: var(--light-bg);
			--mg: var(--light-mg);
			--text: var(--light-text);
			--cell: var(--light-cell);
		}
	}
	:root {
		background-color: var(--bg);
	}
	main {
		text-align: center;
		padding: 1em;

		margin: 0 auto;
	}

	h1 {
		color: var(--text);
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 800;
	}

	p {
		color: var(--text);
	}

</style>