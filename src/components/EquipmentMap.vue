<template>
  <div class="container" style="height: 600px; width: 100%">
    <div style="height: 200px; overflow: auto">
      <p>First marker is placed at {{ withPopup.lat }}, {{ withPopup.lng }}</p>
      <p>Center is at {{ currentCenter }} and the zoom is: {{ currentZoom }}</p>
    </div>
    <l-map
      class="map"
      :zoom="zoom"
      :center="center"
      :options="mapOptions"
      style="height: 80%"
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate"
    >
      <l-tile-layer :url="url" :attribution="attribution" />
      <l-marker
        v-for="(equipment, index) in jsonData"
        :key="index"
        :lat-lng="
          latLgn(equipment.positions[index].lat, equipment.positions[index].lon)
        "
      >
        <l-popup>
          <div @click="innerClick">I am a popup</div>
        </l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";
import jsonData from "../static/data/equipmentPositionHistory.json";

export default {
  name: "Example",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
  },
  data() {
    return {
      jsonData,
      zoom: 11,
      center: latLng(-19.066661, -45.96405),
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      withPopup: latLng(-19.126536, -45.947756),
      currentZoom: 13,
      currentCenter: latLng(-19.126536, -45.947756),
      showParagraph: false,
      mapOptions: {
        zoomSnap: 0.5,
      },
    };
  },
  methods: {
    latLgn(lat, lng) {
      return latLng(lat, lng);
    },
    zoomUpdate(zoom) {
      this.currentZoom = zoom;

      const data = this.jsonData.map((item) => {
        let lat = item.positions[0].lat;
        let lon = item.positions[0].lon;
        console.log(`The lat is: ${lat} and the long is: ${lon}`);
      });

      console.log(data);
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    innerClick() {
      alert("Click!");
    },
  },
};
</script>

<style scoped>
.container {
  height: 600px;
  width: 100%;
}

.map {
  height: 80%;
}
</style>
