<template>
  <div class="container max-w-7xl mx-auto" id="app">
    <Header />
    <EquipmentMap />
  </div>
</template>

<script>
import EquipmentMap from "./components/EquipmentMap.vue";
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    EquipmentMap,
    Header,
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
