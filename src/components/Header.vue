<template>
    <header class="relative" >
        <div class="expanded-width overflow-x-hidden relative">
            <div class="bg-gray-400 background-image bg-auto md:bg-cover bg-center">
              <div class="container max-w-7xl mx-auto text-white pt-10 w-4/5">
                <h1 class="text-3xl md:text-4xl font-bold">Acompanhamento Florestal</h1>
                <p class="max-w-lg text-sm mt-5">Aplicação em Vue para visualizar dados de equipamentos em uma operação florestal.
                </p>
              </div>
            </div>
        </div>
        <div class="flex justify-center md:justify-end mr-0 md:mr-5 xl:mr-0">
          <div class="absolute logo-container bg-white p-5 rounded-md">
              <img class="w-36" src="../assets/aiko.png" alt="">
          </div>
        </div>
    </header>
</template>

<script>
export default {};
</script>


<style scoped>
header {
  font-family: "Poppins", sans-serif;
}

.position-image {
  bottom: -30px;
}

.expanded-width {
  width: 100vw;
  position: relative;
  margin-left: -50vw;
  left: 50%;
}

@media (max-width: 768px) {
  .background-image {
    height: 290px !important;
  }
}

.background-image {
  background-image: url(../assets/liquid-bg.svg);
  height: 230px;
}

.test {
  overflow-x: hidden;
}

.logo-container {
  bottom: -35px;
}
</style>