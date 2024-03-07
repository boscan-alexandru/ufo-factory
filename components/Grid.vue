<template>
  <div
    class="grid bg-gradient-to-br from-sky-600 to-blue-500 w-[2000px] h-[2000px] overflow-auto"
  >
    <div
      class="absolute inset-0 bg-[url('/grid.svg')] opacity-20 bg-[bottom_1px_center] dark:bg-grid-slate-400/[0.05] dark:bg-bottom dark:border-b dark:border-slate-100/40"
    ></div>
    <div class="absolute flex justify-center w-screen top-10">
      <!-- <h1 class="font-sketch text-8xl text-white">UFO FACTORY</h1> -->
      <input
        type="text"
        class="font-sketch text-8xl text-center text-white z-40 border-0 bg-transparent w-fit focus-within:outline-none"
        v-model="text"
      />
    </div>
    <DraggableImage
      :position_left="init_pos_x"
      :position_top="init_pos_y"
      image_url="/ufo.svg"
    />
  </div>
</template>

<script setup>
const text = ref("UFO FACTORY");

const init_pos_x = ref(0);
const init_pos_y = ref(0);

onMounted(() => {
  init_pos_x.value = window.innerWidth / 2.8;
  init_pos_y.value = 200;
});
</script>

<style scoped></style>
