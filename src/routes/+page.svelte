<script lang="ts">
	import Spinner from '$lib/components/spinner.svelte';

	let address: string = $state('');
	let hidden: boolean = $state(false);
	let data: string = $state('');
	let respStatus: number | undefined = $state();
	let success: boolean | undefined = $state();
	let timeoutError: boolean = $state(false);

	const regex = /^(ban_[A-Za-z0-9]+|[A-Za-z0-9]+\.(?:ban|jtv|mictest))$/;

	async function submit() {
		if (regex.test(address)) {
			hidden = true;

			const timeout = setTimeout(() => {
				if (success === undefined) {
					timeoutError = true;
					success = false;
				}
			}, 15000);

			fetch(`https://faucetapi.iamgabriel.dev/?address=${address}`)
				.then((response) => {
					console.log(response);
					if (!response.ok) success = false;
					else success = true;
					respStatus = response.status;
					return response.text();
				})
				.then((dataResp) => {
					data = dataResp;
				})
				.catch((error) => {
					console.error('Error:', error);
				});
		}
	}
</script>

{#if !hidden}
	<form class="my-4 flex justify-center">
		<input
			placeholder="ban_ address or BNS Domain"
			class="mx-2 w-64 bg-slate-800"
			bind:value={address}
		/>
		<button class="cursor-pointer" onclick={submit}>Give banano</button>
	</form>
{/if}

{#if hidden && success === undefined}
	<Spinner />
{/if}

{#if timeoutError}
	<p class="text-4xl">
		Error. The request timed out. If the problem persists, contact @iamgabrieltv on Discord.
	</p>
{/if}

{#if success}
	<p class="text-4xl">
		Success! Hash: <a class="underline" href="https://creeper.banano.cc/hash/{data}">{data}</a>
	</p>
{/if}

{#if (!success && success !== undefined && respStatus === 400) || respStatus === 500}
	<p class="text-4xl">
		Error. Check if the address is correct. If the problem persists, contact @iamgabrieltv on
		Discord. Code: {respStatus}
	</p>
{/if}

{#if !success && success !== undefined && respStatus === 403}
	<p class="text-4xl">
		Error. It seems that you already claimed today. The list resets at 12:00 AM Europe/Berlin. Code: {respStatus}
	</p>
{/if}

<div class="h-fit w-full justify-center gap-8 md:flex">
	<div>
		<p class="text-xl font-bold">Donate to me</p>
		<p class="text-wrap break-all">
			ban_3j95e6bkhp8zyg8pfthnykcqywq7w8edu5d9xa4gbr38ggxwawip39ibx7hr
		</p>
		<p>iamgabrieltv.ban</p>
	</div>
	<div class="my-8 md:my-0">
		<p class="text-xl font-bold">Donate to the faucet</p>
		<p class="text-wrap break-all">
			ban_3iuhae1nq4e1k4r59h4h3p16x5m5c7czmuetm81xmkjeqensxyk3irocz9yy
		</p>
	</div>
</div>
