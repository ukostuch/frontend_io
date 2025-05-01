<template>
  <div id="map" class="w-full h-96 rounded-lg shadow-md"></div>
</template>

<script>
import L from "leaflet";

export default {
  name: "MapOffice",
  mounted() {
    const map = L.map("map").setView(
      [50.061383110585865, 19.936358846085042],
      15
    ); // 15 - zoom scale

    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      attribution:
        '&copy; <a href="https://osm.org/copyright">OpenStreetMap</a>',
    }).addTo(map);

    const marker = L.marker([50.061383110585865, 19.936358846085042]).addTo(
      map
    );
    marker
      .bindPopup(
        "<b>Main Lost and Found Office</b><br>ul.Rynek Główny 1, 30-000 Kraków"
      )
      .openPopup();
  },
};
</script>

<style scoped>
#map {
  height: 400px;
  width: 100%;
}
</style>
