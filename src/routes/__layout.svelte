<script lang="ts">
	import '../app.css';
</script>

<main>
	<slot />
</main>

<footer class="p-4 footer bg-base-300 text-base-content footer-center">
	<div>
	  <p>Used in conjuction with <a class="no-underline hover:underline text-sky-400" href="https://ntfy.sh/">Ntfy</a></p>
	</div>
  </footer>

<style>
	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 1024px;
		height: 100%;
		margin: 0 auto;
		box-sizing: border-box;
	}
</style>
