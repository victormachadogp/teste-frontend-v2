<template>
     <div class="mb-10 lg:mb-0 lg:order-2" >   
        <div class="max-h-48 sm:max-h-96 w-full lg:w-96 overflow-y-auto py-3 scroll-style rounded-md bg-white border history-container">
        <h2 class="text-center text-2xl font-medium text-gray-600 ">Histórico</h2>
        <div class="border-b border-gray-300 my-3"></div>
        <div v-if="currentEquipment.id" class="flex justify-center">
            <p><span class="font-semibold text-gray-700">{{currentEquipment.modelName}}</span> - {{currentEquipment.name}}</p>
        </div>
        <div v-if="!currentEquipment.id">
          <div class="flex justify-center opacity-60">
            <img class="mt-0 hidden md:block md:mt-3 " src="@/assets/decoration.svg" alt="">
          </div>
            <p class="text-xs mt-3 mb-3 mx-3 md:mx-0 text-gray-500 text-center italic">Clique em um marcador no mapa para visualizar o histórico.</p>
          </div>
          
          <div class="my-4 text-center max-w-xs mx-auto v-for-container rounded-md bg-white border shadow-md " v-for="(item, index) in currentEquipment.statePositionsHistory" :key="index">
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
</template>

<script>
export default {
  props: {
    currentEquipment: {
      type: Object,
      default: null,
    },
  },
};
</script>

<style scoped>
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