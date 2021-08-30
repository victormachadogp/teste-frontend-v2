<template>
    <header class="relative" >
        <div class="expanded-width overflow-x-hidden relative">
            <div class="bg-gray-400 background-image"></div>
        </div>
        <div class="absolute logo-container bg-white p-5 rounded-md">
            <img class="w-36" src="../assets/aiko.png" alt="">
        </div>
    </header>
</template>

<script>
export default {};
</script>

<style scoped>
.position-image {
  bottom: -30px;
}

.expanded-width {
  width: 100vw;
  position: relative;
  margin-left: -50vw;
  left: 49.3%;
}

.background-image {
  background-image: url(../assets/subtle-prism.svg);
  height: 150px;
}

.test {
  overflow-x: hidden;
}

.logo-container {
  bottom: -35px;
}
</style>