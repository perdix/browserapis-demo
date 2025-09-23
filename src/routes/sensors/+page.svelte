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
        
        <div class="text-sm text-slate-700 space-y-1">
        <div><span class="font-medium">Alpha (α):</span> {alpha == null ? '–' : Math.round(alpha)}°</div>
        <div><span class="font-medium">Beta (β):</span> {beta  == null ? '–' : Math.round(beta)}°</div>
        <div><span class="font-medium">Gamma (γ):</span> {gamma == null ? '–' : Math.round(gamma)}°</div>
      </div>
   
    </section>

        <section class="bg-white rounded-4xl p-4 flex flex-col gap-4">
        <h2>Device Motion API</h2>
     <!-- <button onclick={}>Start</button> -->

   
    </section>



</div>