<div class="map-wrap">
  <a href="https://www.maptiler.com" class="watermark"><img
    src="https://api.maptiler.com/resources/logo.svg" alt="MapTiler logo"/></a>
  <div class="map" id="map" bind:this={mapContainer}></div>
</div>

<script>
    import { onMount } from 'svelte'
    import { Map, NavigationControl, Marker } from 'maplibre-gl';
    import 'maplibre-gl/dist/maplibre-gl.css';
  
    let map;
    let mapContainer;
    const initialState = { lng: 139.753, lat: 35.6844, zoom: 14 };

    var options = {
      enableHighAccuracy: true,
      timeout: 5000,
      maximumAge: 0
    };

    function success(pos) {
      var crd = pos.coords;
      const apiKey = 'GHyYucjo1c3lVtuHAnS7';  
      map = new Map({
        container: mapContainer,
        style: `https://api.maptiler.com/maps/streets/style.json?key=${apiKey}`,
        center: [crd.longitude, crd.latitude],
        zoom: initialState.zoom
      });
      map.addControl(new NavigationControl(), 'top-right');
      new Marker({color: "#FF0000"})
      .setLngLat([139.7525,35.6846])
      .addTo(map);
    }

    function error(err) {
      console.warn(`ERROR(${err.code}): ${err.message}`);
    }

    onMount(() => {
      navigator.geolocation.getCurrentPosition(success, error, options);
    });
  </script>
  
  <style>
  
    .map-wrap {
      position: relative;
      width: 100%;
      height: calc(100vh - 77px); /* calculate height of the screen minus the heading */
    }
  
    .map {
      position: absolute;
      width: 100%;
      height: 100%;
    }
  
    .watermark {
      position: absolute;
      left: 10px;
      bottom: 10px;
      z-index: 999;
    }
  </style>