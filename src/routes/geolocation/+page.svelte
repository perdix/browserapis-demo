<script>
    let currentPosition = $state(null);
    let watchedPosition = $state(null);
    let loading = $state(false);
    let error = $state(null);
    let watchError = $state(null);
    let watchId = $state(0);


    function getLocation() {
        loading = true;
        error = null;
        
        navigator.geolocation.getCurrentPosition(
            (position) => {
                console.log(position);
                currentPosition = position;
                loading = false;
            },
            (err) => {
                console.error("Geolocation Fehler:", err.message);
                error = err.message;
                loading = false;
            },
            {
                enableHighAccuracy: true,
            }
        );
    }

    function watchPosition() {
        if (watchId) {
            navigator.geolocation.clearWatch(watchId);
            watchId = 0;
            watchedPosition = null;
        } else {
             watchId = navigator.geolocation.watchPosition(
            (position) => {
                console.log("Update:", position)
                watchedPosition = position;
            },
            (err) => {
                console.error("Geolocation Fehler:", err.message);
                watchError = err.message;
            },
                {
                    enableHighAccuracy: true,
                }
        );
        }
         
    }
   
</script>

<div class="w-full h-full p-4 flex flex-col gap-4">
    <h1 class="text-4xl">Geolocation API</h1>

    <section class="bg-white rounded-4xl p-4 flex flex-col gap-4">

        <div class="flex justify-between items-center">
            <h2 class="text-xl">getCurrentPosition()</h2>

                <button 
                    onclick={getLocation}
                    class="bg-blue-500 text-white px-4 py-2 rounded"
                    disabled={loading}
                >
                    {loading ? 'Ermittle Standort...' : 'Standort abrufen'}
                </button>
        </div>
 

        {#if error}
            <p class="text-red-500">Fehler: {error}</p>
        {:else if currentPosition}
            <div class="space-y-2">
                <!-- Basis-Koordinaten -->
                <div class="bg-gray-100 p-3 rounded">
                    <h3 class="font-semibold">Koordinaten</h3>
                    <p><strong>Latitude / Longitude</strong> {currentPosition.coords.latitude.toFixed(6)} {currentPosition.coords.longitude.toFixed(6)}</p>
                    <p><strong>Accuracy:</strong> {currentPosition.coords.accuracy} Meter</p>
                </div>

                <!-- Höhe (falls verfügbar) -->
                <div class="bg-gray-100 p-3 rounded">
                    <h3 class="font-semibold">Höhe</h3>
                    {#if currentPosition.coords.altitude !== null}
                        <p><strong>Altitude:</strong> {currentPosition.coords.altitude.toFixed(2)} Meter</p>
                    {:else}
                        <p><strong>Altitude:</strong> Nicht verfügbar</p>
                    {/if}
                    
                    {#if currentPosition.coords.altitudeAccuracy !== null}
                        <p><strong>Altitude Accuracy:</strong> {currentPosition.coords.altitudeAccuracy} Meter</p>
                    {:else}
                        <p><strong>Altitude Accuracy:</strong> Nicht verfügbar</p>
                    {/if}
                </div>

                <!-- Bewegung (falls verfügbar) -->
                <div class="bg-gray-100 p-3 rounded">
                    <h3 class="font-semibold">Bewegung</h3>
                    {#if currentPosition.coords.heading !== null}
                        <p><strong>Heading:</strong> {currentPosition.coords.heading.toFixed(1)}° (Richtung)</p>
                    {:else}
                        <p><strong>Heading:</strong> Nicht verfügbar (keine Bewegung)</p>
                    {/if}
                    
                    {#if currentPosition.coords.speed !== null}
                        <p><strong>Speed:</strong> {currentPosition.coords.speed.toFixed(2)} m/s</p>
                        <p><strong>Speed (km/h):</strong> {(currentPosition.coords.speed * 3.6).toFixed(1)} km/h</p>
                    {:else}
                        <p><strong>Speed:</strong> Nicht verfügbar</p>
                    {/if}
                </div>

                <!-- Zeitstempel -->
                <div class="bg-gray-100 p-3 rounded">
                    <h3 class="font-semibold">Zeitstempel</h3>
                    <p><strong>Timestamp:</strong> {new Date(currentPosition.timestamp).toLocaleString('de-DE')}</p>
                    <p><strong>Unix Timestamp:</strong> {currentPosition.timestamp}</p>
                </div>

                <!-- Debug: Komplettes Objekt -->
                <details class="bg-gray-50 p-3 rounded">
                    <summary class="font-semibold cursor-pointer">Komplettes Position-Objekt (Debug)</summary>
                    <pre class="mt-2 text-xs overflow-auto">{JSON.stringify(currentPosition, null, 2)}</pre>
                </details>
            </div>
        {/if}
    </section>


    <section class="bg-white rounded-4xl p-4 flex flex-col gap-4">
         <div class="flex justify-between items-center">
            <h2 class="text-xl">watchPosition()</h2>

                <button 
                    onclick={watchPosition}
                    class="bg-blue-500 text-white px-4 py-2 rounded"
                >
                    {watchId ? 'Stop' : 'Start'}
                </button>
         </div>

         {#if watchError}
            <p class="text-red-500">Fehler: {error}</p>
        {:else if watchedPosition}
            <div class="space-y-2">
                <!-- Basis-Koordinaten -->
                <div class="bg-gray-100 p-3 rounded">
                    <h3 class="font-semibold">Koordinaten</h3>
                    <p><strong>Latitude / Longitude</strong> {watchedPosition.coords.latitude.toFixed(6)} {watchedPosition.coords.longitude.toFixed(6)}</p>
                    <p><strong>Accuracy:</strong> {watchedPosition.coords.accuracy} Meter</p>
                </div>

                <!-- Höhe (falls verfügbar) -->
                <div class="bg-gray-100 p-3 rounded">
                    <h3 class="font-semibold">Höhe</h3>
                    {#if watchedPosition.coords.altitude !== null}
                        <p><strong>Altitude:</strong> {watchedPosition.coords.altitude.toFixed(2)} Meter</p>
                    {:else}
                        <p><strong>Altitude:</strong> Nicht verfügbar</p>
                    {/if}
                    
                    {#if watchedPosition.coords.altitudeAccuracy !== null}
                        <p><strong>Altitude Accuracy:</strong> {watchedPosition.coords.altitudeAccuracy} Meter</p>
                    {:else}
                        <p><strong>Altitude Accuracy:</strong> Nicht verfügbar</p>
                    {/if}
                </div>

                <!-- Bewegung (falls verfügbar) -->
                <div class="bg-gray-100 p-3 rounded">
                    <h3 class="font-semibold">Bewegung</h3>
                    {#if watchedPosition.coords.heading !== null}
                        <p><strong>Heading:</strong> {watchedPosition.coords.heading.toFixed(1)}° (Richtung)</p>
                    {:else}
                        <p><strong>Heading:</strong> Nicht verfügbar (keine Bewegung)</p>
                    {/if}
                    
                    {#if watchedPosition.coords.speed !== null}
                        <p><strong>Speed:</strong> {watchedPosition.coords.speed.toFixed(2)} m/s</p>
                        <p><strong>Speed (km/h):</strong> {(watchedPosition.coords.speed * 3.6).toFixed(1)} km/h</p>
                    {:else}
                        <p><strong>Speed:</strong> Nicht verfügbar</p>
                    {/if}
                </div>

                <!-- Zeitstempel -->
                <div class="bg-gray-100 p-3 rounded">
                    <h3 class="font-semibold">Zeitstempel</h3>
                    <p><strong>Timestamp:</strong> {new Date(watchedPosition.timestamp).toLocaleString('de-DE')}</p>
                    <p><strong>Unix Timestamp:</strong> {watchedPosition.timestamp}</p>
                </div>

                <!-- Debug: Komplettes Objekt -->
                <details class="bg-gray-50 p-3 rounded">
                    <summary class="font-semibold cursor-pointer">Komplettes Position-Objekt (Debug)</summary>
                    <pre class="mt-2 text-xs overflow-auto">{JSON.stringify(watchedPosition, null, 2)}</pre>
                </details>
            </div>
        {/if}
           
    </section>


</div>