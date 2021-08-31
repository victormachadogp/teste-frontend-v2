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
            <div class="text-center" @click="innerClick()">
              {{ equipmentStateInfo }}
            </div>
          </l-popup>
        </l-marker>
      </l-map>
    </div>

    <div class="mb-10 lg:mb-0 lg:order-2" >
        <!-- <p>First marker is placed at {{ withPopup.lat }}, {{ withPopup.lng }}</p>
        <p>Center is at {{ currentCenter }} and the zoom is: {{ currentZoom }}</p> -->
        
        <div class="max-h-48 sm:max-h-96 w-full lg:w-96 overflow-y-auto py-3 scroll-style rounded-md bg-white border history-container">
        <h2 class="text-center text-2xl font-medium text-gray-600 ">Histórico</h2>
        <div class="border-b border-gray-300 my-3"></div>
          <p v-if="!ishistoryPositionOpen" class="text-xs mt-5 text-gray-500 text-center italic">Clique em um marcador no mapa para visualizar o histórico.</p>

          <div class="my-4 text-center max-w-xs mx-auto v-for-container bg-white border shadow-md " v-for="(item, index) in currentStatePositionsHistory" :key="index">
            <p class="text-sm py-1 text-gray-600">{{ item.date }}</p>
            <div class="flex justify-between">
              <div class="flex items-center">
                <div class="text-small text-white bg-variant-color-1 flex items-center justify-center rounded-bl-md w-10 h-7">LAT</div>
                <span class="text-sm ml-3">{{ item.lat }}</span>
              </div>
              <div class="flex items-center">
                <span class="text-sm mr-3">{{ item.lon }}</span>
                <div class="text-small text-white bg-variant-color-2 flex items-center justify-center rounded-br-md w-10 h-7">LON</div>
              </div>
            </div>
          </div>
        </div>
      </div>
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
      ishistoryPositionOpen: false,
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
      this.ishistoryPositionOpen = true;
      this.currentStatePositionsHistory =
        this.equipmentPositionHistory[index].positions;

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

.text-small {
  font-size: 0.65rem;
}

.bg-variant-color-1 {
  background-color: #52bbdc;
}

.bg-variant-color-2 {
  background-color: #3f90cb;
}

.scroll-style::-webkit-scrollbar {
  width: 0.5em;
}

.scroll-style::-webkit-scrollbar-track {
  background-color: rgb(197, 197, 197);
}

.scroll-style::-webkit-scrollbar-thumb {
  background-color: #52bbdc;
}

.history-container {
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.25);
  border-radius: 5px;
}

@media (max-width: 390x) {
  .v-for-container {
    margin-left: 10px;
    margin-right: 10px;
  }
}

@media (min-width: 1024px) {
  .history-container {
    min-height: 600px !important;
  }
}
</style>
