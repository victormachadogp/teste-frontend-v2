<template>
  <div class="container">
    <l-map class="map" :zoom="zoom" :center="center" :options="mapOptions">
      <l-tile-layer :url="url" :attribution="attribution" />
      <l-marker :lat-lng="withPopup">
        <l-popup>
          <div>I am a popup</div>
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
      zoom: 13,
      center: latLng(47.41322, -1.219482),
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      withPopup: latLng(47.41422, -1.219482),
      currentZoom: 13,
      currentCenter: latLng(47.41422, -1.219482),
      showParagraph: false,
      mapOptions: {
        zoomSnap: 0.5,
      },
    };
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
