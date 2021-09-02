<template>
<div class="flex flex-col lg:justify-between lg:flex-row my-16 px-5 lg:px-0">
    <div class="container-map w-full lg:pr-10 mb-10 lg:mb-0 order-2">
      <l-map
        :zoom="zoom"
        :center="center"
        :options="mapOptions"
      >
        <l-tile-layer :url="url" :attribution="attribution" />
        <l-marker
          @click="handleCurrentMarkerClick(index)"
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
            <div class="equipment-state-color" :style="equipmentColor"></div>
            <div class="text-center">
              {{ equipmentStateInfo }}
            </div>
          </l-popup>
        </l-marker>
      </l-map>
    </div>

   <state-history :equipmentModelName="currentEquipmentModelName" :equipmentName="currentEquipmentName" :isHistoryOpen="isHistoryPositionOpen" :currentcurrentStatePositions="currentStatePositionsHistory"/>
   
  </div>

</template>



<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";
import equipmentPositionHistory from "../static/data/equipmentPositionHistory.json";
import equipmentStateHistory from "../static/data/equipmentStateHistory.json";
import equipmentState from "../static/data/equipmentState.json";
import equipments from "../static/data/equipment.json";
import equipmentModel from "../static/data/equipmentModel.json";
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
      equipmentModel,
      equipments,
      equipmentColor: {
        backgroundColor: null,
      },
      currentEquipmentStateId: null,
      isHistoryPositionOpen: false,
      currentStatePositionsHistory: null,
      equipmentStateInfo: null,
      currentEquipmentModelId: null,
      currentEquipmentName: null,
      currentEquipmentModelName: null,
      zoom: 11,
      center: latLng(-19.104946, -45.98877),
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      currentZoom: 11,
      mapOptions: {
        zoomSnap: 0.5,
      },
    };
  },
  methods: {
    latLgn(lat, lng) {
      return latLng(lat, lng);
    },
    handleCurrentMarkerClick(index) {
      this.setCurrentStateId(index);
      this.showCurrentEquipmentState();
      this.toggleHistory();
      this.showEquipmentHistory(index);
      this.formatCurrentEquipmentDate(index);
      this.setCurrentEquipmentInfo(index);
      this.setEquipmentName();
    },
    setCurrentStateId(index) {
      this.currentEquipmentStateId =
        this.equipmentStateHistory[index].states.slice(-1)[0].equipmentStateId;
    },
    showCurrentEquipmentState() {
      this.equipmentState.forEach((item) => {
        if (item.id === this.currentEquipmentStateId) {
          this.equipmentColor.backgroundColor = item.color;
          this.equipmentStateInfo = item.name;
        }
      });
    },
    toggleHistory() {
      this.isHistoryPositionOpen = true;
    },
    showEquipmentHistory(index) {
      this.currentStatePositionsHistory =
        this.equipmentPositionHistory[index].positions;
    },
    formatCurrentEquipmentDate(index) {
      if (
        this.equipmentPositionHistory[index].positions[0].date.endsWith("h")
      ) {
        return;
      } else {
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
    },
    setCurrentEquipmentInfo(index) {
      this.equipments.forEach((item) => {
        if (item.id === this.equipmentStateHistory[index].equipmentId) {
          this.currentEquipmentModelId = item.equipmentModelId;
          this.currentEquipmentName = item.name;
        }
      });
      index;
    },
    setEquipmentName() {
      this.equipmentModel.forEach((equipment) => {
        if (equipment.id === this.currentEquipmentModelId) {
          this.currentEquipmentModelName = equipment.name;
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
