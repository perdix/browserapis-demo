<script>
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<div class="wrapper min-h-screen bg-blue-100 flex flex-col">


<nav class="bg-blue-950 p-4">
	<ul class="flex gap-2 items-center justify-start">
		<li>
			<a href="/" class="text-white">
				Home
			</a>
		</li>
		<li>
			<a href="/geolocation" class="px-4 py-1 rounded-lg bg-white">Geolocation API</a>
		</li>
				<li>
			<a href="/sensors" class="px-4 py-1 rounded-lg bg-white">Device API</a>
		</li>
	</ul>
</nav>

<main class="grow">
	{@render children?.()}
</main>
</div>


