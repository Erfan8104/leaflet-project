<template>
  <div ref="mapContainer" class="w-full h-screen" />
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import L from "leaflet";
import "leaflet/dist/leaflet.css";

const mapContainer = ref<HTMLElement | null>(null);

onMounted(() => {
  if (!mapContainer.value) return;

  const map = L.map(mapContainer.value).setView([36.33, 59.6], 11);

  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: "&copy; OpenStreetMap contributors",
  }).addTo(map);

  L.marker([36.306613, 59.576064]).addTo(map).bindPopup("شرکت ساج ");
});
</script>
