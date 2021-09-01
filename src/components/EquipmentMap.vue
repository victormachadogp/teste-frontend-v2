<template>
<div class="flex flex-col lg:justify-between lg:flex-row my-16 px-5 lg:px-0">
    <div class="container-map w-full lg:pr-10 mb-10 lg:mb-0 order-2">
      <l-map
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
              equipment.positions[equipment.positions.length - 1].lon,
            )
          "
        >
          <l-popup>
            <div class="equipment-state-color" :style="equipColor"></div>
            <div class="text-center">
              {{ equipmentStateInfo }}
            </div>
          </l-popup>
        </l-marker>
      </l-map>
    </div>

   <state-history :isHistoryOpen="isHistoryPositionOpen" :currentcurrentStatePositions="currentStatePositionsHistory"/>
   
  </div>

</template>



<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";
import equipmentPositionHistory from "../static/data/equipmentPositionHistory.json";
import equipmentStateHistory from "../static/data/equipmentStateHistory.json";
import equipmentState from "../static/data/equipmentState.json";
import StateHistory from "./StateHistory.vue";

export default {
  name: "EquipmentMap",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    StateHistory,
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
      isHistoryPositionOpen: false,
      currentStatePositionsHistory: null,
      equipmentStateInfo: null,
      zoom: 11,
      center: latLng(-19.066661, -45.96405),
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      // Apagar esses 3 caras se não for usar
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
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    innerClick() {},
    showState(index) {
      this.isHistoryPositionOpen = true;

      // Mostra os dados no historico
      this.currentStatePositionsHistory =
        this.equipmentPositionHistory[index].positions;

      if (
        this.equipmentPositionHistory[index].positions[0].date.endsWith("h")
      ) {
        return;
      } else {
        // // Tratando dados dos dias
        this.currentStatePositionsHistory.forEach((item) => {
          const date = new Date(item.date).toLocaleDateString("pt-Br", {
            dateStyle: "short",
          });
          const time = new Date(item.date).toLocaleTimeString("pt-Br", {
            timeStyle: "short",
          });

          item.date = `${date} ${time}h`;
        });
      }

      // Devolve o ultimo estado do equipamento que eu clicar
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
.container-map {
  height: 600px;
}

.equipment-state-color {
  width: 10px;
  height: 10px;
  background-color: gray;
  border-radius: 50%;
  margin: 0 auto;
}
</style>
