<script>
	import { browser } from '$app/environment';

	let isOrientaionListening = $state(false);
	let isMotionListening = $state(false);

	let alpha = $state(null); // 0..360 (Kompass)
	let beta = $state(null); // -180..180 (vor/zurück)
	let gamma = $state(null); // -90..90 (links/rechts)

	let acceleration = $state(null);
	let accelerationGravity = $state(null);
	let rotation = $state(null);

	function orient(e) {
		alpha = e.alpha;
		beta = e.beta;
		gamma = e.gamma;
	}

	function motion(e) {
		acceleration = e.acceleration;
		accelerationGravity = e.accelerationIncludingGravity;
		rotation = e.rotationRate;
	}

	async function getOrientation() {
		if (isOrientationListening) {
			isOrientationListening = false;
			alpha = null;
			beta = null;
			gamma = null;
			window.removeEventListener('deviceorientationabsolute', orient);
			return;
		} else {
			isOrientationListening = true;
		}
		window.addEventListener('deviceorientationabsolute', orient);
	}

	async function getMotion() {
		if (isMotionListening) {
			isMotionListening = false;
			acceleration = null;
			accelerationGravity = null;
			rotation = null;
			window.removeEventListener('devicemotion', motion);
		} else {
			isMotionListening = true;
		}
		window.addEventListener('devicemotion', motion);
	}
</script>

<div class="flex h-full w-full flex-col gap-4 p-4">
	<h1 class="text-xl">Device APIs</h1>

	<section class="flex flex-col gap-4 rounded-4xl bg-white p-4">
		<h2>Device Orientation API</h2>

		<div>
			<button
				onclick={getOrientation}
				class="cursor-pointer rounded bg-blue-500 px-4 py-2 text-white hover:bg-blue-600"
			>
				{isOrientaionListening ? 'Stop' : 'Start'}
			</button>
		</div>

		<div class="space-y-1 text-sm text-slate-700">
			<div>
				<span class="font-medium">Alpha (α):</span>
				{alpha == null ? '–' : Math.round(alpha)}°
			</div>
			<div><span class="font-medium">Beta (β):</span> {beta == null ? '–' : Math.round(beta)}°</div>
			<div>
				<span class="font-medium">Gamma (γ):</span>
				{gamma == null ? '–' : Math.round(gamma)}°
			</div>
		</div>
	</section>

	<section class="flex flex-col gap-4 rounded-4xl bg-white p-4">
		<h2>Device Motion API</h2>
		<div>
			<button
				onclick={getMotion}
				class="cursor-pointer rounded bg-blue-500 px-4 py-2 text-white hover:bg-blue-600"
			>
				{isMotionListening ? 'Stop' : 'Start'}
			</button>
		</div>

		<div class="space-y-1 text-sm text-slate-700">
			<div>
				<span class="font-medium">Acceleration:</span>
				{#if acceleration}
					x: {Math.round(acceleration.x * 100) / 100} m/s², y: {Math.round(acceleration.y * 100) /
						100} m/s², z: {Math.round(acceleration.z * 100) / 100} m/s²
				{:else}
					–
				{/if}
			</div>
			<div>
				<span class="font-medium">Acceleration (inkl. Gravitation):</span>
				{#if accelerationGravity}
					x: {Math.round(accelerationGravity.x * 100) / 100} m/s², y: {Math.round(
						accelerationGravity.y * 100
					) / 100} m/s², z: {Math.round(accelerationGravity.z * 100) / 100} m/s²
				{:else}
					–
				{/if}
			</div>
			<div>
				<span class="font-medium">Rotation Rate:</span>
				{#if rotation}
					alpha: {Math.round(rotation.alpha * 100) / 100} °/s, beta: {Math.round(
						rotation.beta * 100
					) / 100} °/s, gamma: {Math.round(rotation.gamma * 100) / 100} °/s
				{:else}
					–
				{/if}
			</div>
		</div>
	</section>
</div>
