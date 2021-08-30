<template>
  <div class="container max-w-7xl mx-auto" id="app">
    <EquipmentMap />
  </div>
</template>

<script>
import EquipmentMap from "./components/EquipmentMap.vue";

export default {
  name: "App",
  components: {
    EquipmentMap,
  },
};
</script>

<style>
@media (min-width: 1152px) {
  .expanded-width {
    width: 100vw;
    position: relative;
    margin-left: -50vw;
    left: 50%;
  }
}
</style>
