<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<main class="p-4">
	<h1 class="text-center text-7xl md:text-9xl">IamFaucet</h1>

	{@render children()}
</main>
