<script>
    let alpha = $state(null);  // 0..360 (Kompass)
    let beta  = $state(null);  // -180..180 (vor/zurück)
    let gamma = $state(null);  // -90..90 (links/rechts)

      $effect(() => {
            if (!browser || !granted) return;
           

            const onOrient = (e) => {
                alpha = e.alpha;
                beta  = e.beta;
                gamma = e.gamma;
            };

            window.addEventListener('deviceorientation', onOrient, { passive: true });
            return () => window.removeEventListener('deviceorientation', onOrient);
  });
   
</script>

<div class="w-full h-full p-4 flex flex-col gap-4">
    <h1 class="text-xl">Device APIs</h1>


    <section class="bg-white rounded-4xl p-4 flex flex-col gap-4">
        <h2>Device Orientation API</h2>
        
        <details class="text-sm text-slate-600">
        <summary class="cursor-pointer select-none">Technische Details</summary>
        <ul class="list-disc ps-5 mt-2 space-y-1">
            <li>α (alpha) – Rotation um Z-Achse → Kompassrichtung (0–360°)</li>
            <li>β (beta) – Rotation um X-Achse → Neigung vor/zurück (−180..180)</li>
            <li>γ (gamma) – Rotation um Y-Achse → Neigung links/rechts (−90..90)</li>
            <li>Nur auf HTTPS und echten Mobilgeräten zuverlässig</li>
        </ul>
        </details>
   
    </section>

        <section class="bg-white rounded-4xl p-4 flex flex-col gap-4">
        <h2>Device Motion API</h2>
     <!-- <button onclick={}>Start</button> -->

   
    </section>



</div>