<template>
  <div ref="mapContainer" class="w-full h-screen" />
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import L from "leaflet";
import "leaflet/dist/leaflet.css";
import points from "../data/points.json";
import type { Point } from "../types/point";
import markerIcon from "../assets/marker.svg";
import NeshanMap from "@neshan-maps-platform/leaflet";
const neshanApiKey = import.meta.env.VITE_NESHAN_API_KEY;

const mapContainer = ref<HTMLElement | null>(null);

const customIcon = L.icon({
  iconUrl: markerIcon,
  iconSize: [36, 36],
  iconAnchor: [18, 36],
});

onMounted(() => {
  if (!mapContainer.value) return;
  console.log("API KEY:", neshanApiKey);
  console.log("NeshanMap:", NeshanMap);
  console.log("L.Map =", L.Map);
  console.log("NeshanMap.Map =", (NeshanMap as any).Map);
  const map = new (NeshanMap as any).Map(mapContainer.value, {
    key: neshanApiKey,
    maptype: "dreamy",
    poi: true,
    traffic: false,
    center: [36.33, 59.6],
    zoom: 11,
  });

  (points as Point[]).forEach((point) => {
    L.marker([point.lat, point.lng], {
      icon: customIcon,
    }).addTo(map).bindPopup(`
       <div class="min-w-[200px]">
            <h3 class="font-bold text-lg">${point.name}</h3>
            <p class="text-sm text-gray-600">
               ${point.description}
            </p>
      </div>

`);
  });

  const bounds = points.map(
    (point) => [point.lat, point.lng] as [number, number],
  );

  map.fitBounds(bounds, {
    padding: [20, 20],
  });
  console.log(map.getBounds());
});
</script>
