<template>
  <div>
    <input type="text" v-model="ipAddress" placeholder="Digite o endereço IP" />
    <button @click="getLocation()">Buscar Localização</button>
    <l-map :center="center" :zoom="zoom" class="tamanhomapa">
      <l-tile-layer :url="url"></l-tile-layer>
      <l-marker :lat-lng="location"></l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import axios from "axios";

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },
  data() {
    return {
      center: [0, 0],
      zoom: 20,
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      location: [0, 0],
      ipAddress: "", // IP padrão
    };
  },
  methods: {
    async getLocation() {
      try {
        const apiKey = "88391B33F85CFDED27A7FABA3E1F1ED6";

        const response = await axios.get(
          `https://api.ip2location.io/?key=${apiKey}&ip=${this.ipAddress}`
        );
        const { latitude, longitude } = response.data;
        this.location = [latitude, longitude];
        this.center = [latitude, longitude];
      } catch (error) {
        console.error("Error fetching location:", error);
      }
    },
  },
};
</script>

<style scoped>
.tamanhomapa {
  height: 50px;
  weight: 50px;
}
</style>
