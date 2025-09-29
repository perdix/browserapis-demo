<script>
	let copyInput = $state();
	let pasteInput = $state();

	async function copy() {
		await navigator.clipboard.writeText(copyInput);
	}

	async function paste() {
		pasteInput = await navigator.clipboard.readText();
	}

	async function share() {
		if (navigator.share) {
			try {
				await navigator.share({
					title: 'Lerne SvelteKit',
					text: 'Hier kannst du Svelte lernen!',
					url: "https://www.youtube.com/watch?v=B2MhkPtBWs4"
				});
				console.log('Daten erfolgreich geteilt');
			} catch (error) {
				console.error('Fehler beim Teilen:', error);
			}
		} else {
			alert('Share API wird von diesem Browser nicht unterstützt.');
		}
	}

	async function notify() {
		if ('Notification' in window) {
			if (Notification.permission === 'granted') {
				new Notification('Hallo! Das ist eine Benachrichtigung.');
			} else if (Notification.permission !== 'denied') {
				let permission = await Notification.requestPermission();
				if (permission === 'granted') {
					new Notification('Danke! Du wirst jetzt benachrichtigt.');
				}
			}
		} else {
			alert('Notification API wird von diesem Browser nicht unterstützt.');
		}
	}
</script>

<div class="flex h-full w-full flex-col gap-4 p-4">
	<h1 class="text-xl">Austauch von Daten</h1>

	<section class="flex flex-col gap-4 rounded-4xl bg-white p-4">
		<div class="flex items-center justify-between">
			<h2 class="text-lg">Clipboard API</h2>

			<div>
			<input type="text" bind:value={copyInput} >
			<!-- svelte-ignore a11y_consider_explicit_label -->
			<button onclick={copy} class="border p-2 cursor-pointer">Copy</button>

			
			<!-- svelte-ignore a11y_consider_explicit_label -->
			<button onclick={paste} class="border p-2 cursor-pointer">Paste</button>
			<input type="text" bind:value={pasteInput} >
			</div>
			
		</div>

	
	</section>

	<section class="flex flex-col gap-4 rounded-4xl bg-white p-4">
		<div class="flex items-center justify-between">
			<h2 class="text-lg">Share API</h2>


			<button onclick={share} class="border p-2 cursor-pointer">Share</button>
		</div>


	</section>

		<section class="flex flex-col gap-4 rounded-4xl bg-white p-4">
		<div class="flex items-center justify-between">
			<h2 class="text-lg">Notification API</h2>


			<button onclick={notify} class="border p-2 cursor-pointer">Notifiy me</button>
		</div>


	</section>
</div>
