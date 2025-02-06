<script lang="ts">
	let address: string = $state('');
	let hidden: boolean = $state(false);
	let data: string = $state('');
	let respStatus: number | undefined = $state();
	let success: boolean | undefined = $state();

	async function submit() {
		hidden = true;
		fetch(`http://localhost:3000/?address=${address}`)
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
</script>

{#if !hidden}
	<form class="my-4 flex justify-center">
		<input
			placeholder="ban_ address or BNS Domain"
			class="mx-2 w-64 bg-slate-800"
			bind:value={address}
		/>
		<button onclick={submit}>Give banano</button>
	</form>
{/if}

{#if success}
	<p>Success! Hash: <a href="https://creeper.banano.cc/hash/{data}">{data}</a></p>
{/if}

{#if !success && success !== undefined && respStatus !== 403}
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
