<template>
  <div class="container">
    <div class="text-field">
      <p>First marker is placed at {{ withPopup.lat }}, {{ withPopup.lng }}</p>
      <p>Center is at {{ currentCenter }} and the zoom is: {{ currentZoom }}</p>

      <div v-for="(item, index) in currentStatePositionsHistory" :key="index">
        <span>{{ item.date }}</span>
        <span>{{ item.lat }}</span>
        <span>{{ item.lon }}</span>
      </div>
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
          <div class="equipment-state-color" :style="equipColor"></div>
          <div class="equipment-state-text" @click="innerClick()">
            {{ equipmentStateInfo }}
          </div>
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
      currentEquipmentStateId: null,
      currentStatePositionsHistory: [],
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

      console.log(equipmentPositionHistory);
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    innerClick() {},
    showState(index) {
      this.currentStatePositionsHistory =
        this.equipmentPositionHistory[index].positions;

      // // Date
      // this.currentEquipmentStateHistoryDate =
      //   this.equipmentPositionHistory[index].positions[0].date;
      // // Lat
      // this.currentEquipmentStateHistoryLat =
      //   this.equipmentPositionHistory[index].positions[0].lat;
      // // Long
      // this.currentEquipmentStateHistoryLon =
      //   this.equipmentPositionHistory[index].positions[0].lon;

      // Codigo que me devolve o ultimo estado do equipamento que eu clicar
      this.currentEquipmentStateId =
        this.equipmentStateHistory[index].states.slice(-1)[0].equipmentStateId;

      // Atribuir dados para cada equipamento
      this.equipmentState.forEach((item) => {
        if (item.id === this.currentEquipmentStateId) {
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

.equipment-state-color {
  width: 10px;
  height: 10px;
  background-color: gray;
  border-radius: 50%;
  margin: 0 auto;
}

.equipment-state-text {
  text-align: center;
}
</style>
