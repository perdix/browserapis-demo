<script>
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<div class="wrapper flex min-h-screen flex-col bg-blue-100">
	<nav class="bg-blue-950 p-4">
		<ul class="flex items-center justify-start gap-2 flex-col sm:flex-row">
			<li>
				<a href="/" class="text-white"> Home </a>
			</li>
			<li>
				<a href="/geolocation" class="rounded-lg bg-white px-4 py-1">Geolocation API</a>
			</li>
			<li>
				<a href="/sensors" class="rounded-lg bg-white px-4 py-1">Device APIs</a>
			</li>
			<li>
				<a href="/daten" class="rounded-lg bg-white px-4 py-1">Daten APIs</a>
			</li>
		</ul>
	</nav>

	<main class="grow">
		{@render children?.()}
	</main>
</div>
