<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<main class="p-4">
	<h1 class="text-center text-7xl md:text-9xl">IamFaucet</h1>

	{@render children()}
</main>
