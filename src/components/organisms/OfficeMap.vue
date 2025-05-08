<template>
  <div id="map" class="w-full h-96 rounded-md shadow-md"></div>
</template>

<script>
import L from "leaflet";
import axios from "axios";
import { nextTick } from "vue";

export default {
  name: "OfficeMap",
  props: {
    offices: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      map: null,
      markers: [],
    };
  },

  mounted() {
    nextTick(() => {
      const mapContainer = document.getElementById("map");
      if (!mapContainer) {
        console.error("Map container not found!");
        return;
      }

      this.map = L.map(mapContainer).setView([50.0614, 19.9364], 11);
      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution:
          '&copy; <a href="https://osm.org/copyright">OpenStreetMap</a>',
      }).addTo(this.map);

      if (this.map) {
        this.updateMap(this.offices);
      }

      this.map.on("zoomend", this.closePopupsOnZoom);
    });
  },

  watch: {
    offices: {
      handler(newOffices) {
        if (newOffices.length > 0 && this.map) {
          this.updateMap(newOffices);
        }
      },
      immediate: true,
      deep: true,
    },
  },

  methods: {
    async updateMap(offices) {
      if (!this.map) return;

      this.map.eachLayer((layer) => {
        if (layer instanceof L.Popup) {
          this.map.closePopup(layer);
        }
      });

      this.markers.forEach((marker) => this.map.removeLayer(marker));
      this.markers = [];

      const locations = [];

      for (const office of offices) {
        const coords = await this.geocodeAddress(office.address);
        if (coords && coords.length === 2) {
          locations.push({ office, coords });
        }
      }

      const bounds = [];

      for (const { office, coords } of locations) {
        const marker = L.marker(coords).addTo(this.map);
        marker.bindPopup(
          `<b>${office.address}</b><br><i>District: ${office.district.name}</i><br><i>Phone: ${office.phoneNumber}</i>`
        );
        this.markers.push(marker);
        bounds.push(coords);
      }

      if (bounds.length > 0) {
        this.map.fitBounds(bounds);
      }
    },

    async geocodeAddress(address) {
      if (!address) {
        console.warn("Empty address!");
        return null;
      }

      try {
        const response = await axios.get(
          `https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(
            address
          )}`
        );

        if (response.data.length > 0) {
          const { lat, lon } = response.data[0];
          return [parseFloat(lat), parseFloat(lon)];
        }
        console.warn(`No results for adress: ${address}`);
        return null;
      } catch (error) {
        console.error("Geocode error:", error);
        return null;
      }
    },

    closePopupsOnZoom() {
      this.map.closePopup();
    },
  },
};
</script>
