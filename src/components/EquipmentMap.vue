<template>
  <div class="container">
    <div class="text-field">
      <p>First marker is placed at {{ withPopup.lat }}, {{ withPopup.lng }}</p>
      <p>Center is at {{ currentCenter }} and the zoom is: {{ currentZoom }}</p>
    </div>
    <l-map
      class="map"
      :zoom="zoom"
      :center="center"
      :options="mapOptions"
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate"
    >
      <l-tile-layer :url="url" :attribution="attribution" />
      <l-marker
        @click="showState(index)"
        v-for="(equipment, index) in equipmentPositionHistory"
        :key="index"
        :lat-lng="
          latLgn(
            equipment.positions[equipment.positions.length - 1].lat,
            equipment.positions[equipment.positions.length - 1].lon
          )
        "
      >
        <l-popup>
          <div @click="innerClick()">{{ equipmentStateInfo }}</div>
          <div class="aColor" :style="equipColor"></div>
        </l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";
import equipmentPositionHistory from "../static/data/equipmentPositionHistory.json";
import equipmentStateHistory from "../static/data/equipmentStateHistory.json";
import equipmentState from "../static/data/equipmentState.json";

export default {
  name: "EquipmentMap",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
  },
  data() {
    return {
      equipmentPositionHistory,
      equipmentStateHistory,
      equipmentState,
      equipColor: {
        backgroundColor: null,
      },
      // Mude isso
      equipmentActualState: "Um estado aqui",
      equipmentStateInfo: null,
      zoom: 11,
      center: latLng(-19.066661, -45.96405),
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      withPopup: latLng(-19.126536, -45.947756),
      currentZoom: 11,
      currentCenter: latLng(-19.126536, -45.947756),
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
      console.log;

      // console.log(items);
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    innerClick() {},
    showState(index) {
      // Codigo que me devolve o ultimo estado do equipamento que eu clicar
      this.equipmentActualState =
        this.equipmentStateHistory[index].states.slice(-1)[0].equipmentStateId;

      // Atribuir dados para cada equipamento
      this.equipmentState.forEach((item) => {
        if (item.id === this.equipmentActualState) {
          this.equipColor.backgroundColor = item.color;
          this.equipmentStateInfo = item.name;
        }
      });
    },
  },
};
</script>

<style scoped>
.container {
  height: 600px;
  width: 100%;
}

.text-field {
  height: 200px;
  overflow: auto;
}

.map {
  height: 80%;
}

.aColor {
  width: 10px;
  height: 10px;
  background-color: gray;
  border-radius: 50%;
}
</style>
