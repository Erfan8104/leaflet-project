<template>
  <div ref="mapContainer" class="w-full h-screen" />
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import L from "leaflet";
import "leaflet/dist/leaflet.css";
import points from "../data/points.json";
import type { Point } from "../types/point";

const mapContainer = ref<HTMLElement | null>(null);

onMounted(() => {
  if (!mapContainer.value) return;

  const map = L.map(mapContainer.value).setView([36.33, 59.6], 11);

  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: "&copy; OpenStreetMap contributors",
  }).addTo(map);

  (points as Point[]).forEach((point) => {
    L.marker([point.lat, point.lng]).addTo(map).bindPopup(point.name);
  });
});
</script>
