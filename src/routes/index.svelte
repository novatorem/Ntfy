<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	let topic = 'atlas-nova';
	let message = '';

	let title = '';
	let tags = '';
	let click = '';

	let priorities = [
		{ id: 'Minimum priority', value: 1 },
		{ id: 'Low priority', value: 2 },
		{ id: 'Medium priority', value: 3 },
		{ id: 'High priority', value: 4 },
		{ id: 'Maximum priority', value: 5 }
	];

	let priority = priorities[2];

	class Result {
		id: string;
		time: number;
		event: string;
	}
	let result = new Result();

	async function ntfy() {
		let webhook = 'https://ntfy.sh/' + topic + '/publish?message=' + message.replace(/\s/g, '+');

		title ? (webhook = webhook + '&title=' + title.replace(/\s/g, '+')) : webhook;
		priority ? (webhook = webhook + '&priority=' + priority.value) : webhook;
		tags ? (webhook = webhook + '&tags=' + tags) : webhook;
		click ? (webhook = webhook + '&click=' + click) : webhook;

		const res = await fetch(webhook, { method: 'GET' });
		const json = await res.json();
		result = Object.assign(new Result(), json);
	}
</script>

<svelte:head>
	<title>Ntfy</title>
</svelte:head>

<section class="h-full">
	<div class="navbar mb-2 shadow-lg bg-neutral text-neutral-content rounded-box">
		<div class="flex-1 px-2 mx-2">
			<span class="text-lg font-bold"> NTFY </span>
		</div>

		<a class="flex-none" href="https://ntfy.sh/{topic}" target="_blank">
			<button class="btn btn-square btn-ghost">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					class="inline-block w-6 h-6 stroke-current text-success"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"
					/>
				</svg>
			</button>
		</a>
	</div>
	<div id="ntfy" class="h-2/3">
		<input placeholder="Topic*" class="input input-bordered my-5 w-full" bind:value={topic} />

		<input placeholder="Message" class="input input-bordered my-5 w-full" bind:value={message} />

		<input placeholder="Title" class="input input-bordered my-5 w-full" bind:value={title} />

		<input placeholder="Click URL" class="input input-bordered my-5 w-full" bind:value={click} />

		<div class="w-full indicator">
			<a href="https://ntfy.sh/docs/emojis/" target="_blank">
				<div class="indicator-item badge indicator-middle">?</div>
			</a>
			<input placeholder="Tags" class="input input-bordered my-5 w-full" bind:value={tags} />
		</div>

		<div class="dropdown dropdown-hover my-5 w-full">
			<div tabindex="0" class="btn w-full">{priority.id}</div>
			<ul tabindex="0" class="p-2 shadow menu dropdown-content bg-base-100 rounded-box w-full">
				{#each priorities as item}
					<li>
						<button on:click={() => (priority = item)}>
							{item.id || item.value}
						</button>
					</li>
				{/each}
			</ul>
		</div>

		<button class="btn btn-primary my-5 w-full" on:click={ntfy}>Notify</button>

		{#if result.id != null}
			<div class="w-2/3 shadow stats">
				<div class="stat place-items-center place-content-center">
					<div class="stat-title">ID</div>
					<div class="stat-value">{result.id}</div>
				</div>
				<div class="stat place-items-center place-content-center">
					<div class="stat-title">Time</div>
					<div class="stat-value text-success">{new Date(result.time * 1000).toLocaleString()}</div>
				</div>
				<div class="stat place-items-center place-content-center">
					<div class="stat-title">Event</div>
					<div class="stat-value text-success">{result.event}</div>
				</div>
			</div>
		{/if}
	</div>
</section>

<style>
	#ntfy {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}

	.stats {
		flex-shrink: 0;
		position: fixed;
		bottom: 100px;
	}
</style>
