<template>
  <div
    class="drag-container"
    ref="container"
    @mousedown.middle="startDrag"
    @mousemove="handleDrag"
    @mouseup="endDrag"
  >
    <div
      class="drag-image relative h-fit w-fit"
      ref="image"
      :class="classesByStatus"
      v-click-outside="clickedOutside"
    >
      <template v-if="resizing">
        <div
          class="absolute -left-2 -top-2 h-4 w-4 border border-white hover:cursor-nwse-resize"
        ></div>
        <div
          class="absolute -right-2 -top-2 h-4 w-4 border border-white hover:cursor-nesw-resize"
        ></div>
        <div
          class="absolute -left-2 -bottom-2 h-4 w-4 border border-white hover:cursor-nesw-resize"
        ></div>
        <div
          class="absolute -right-2 -bottom-2 h-4 w-4 border border-white hover:cursor-nwse-resize"
        ></div>
      </template>
      <!-- <img :src="image_url" @click="showResize" /> -->
      <UFO @click="showResize" color="#fff" />
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps, computed } from "vue";

const props = defineProps({
  image_url: {
    type: String,
    default: null,
  },
  position_top: {
    type: Number,
    default: 0,
  },
  position_left: {
    type: Number,
    default: 0,
  },
});

const classesByStatus = computed(() => {
  let classes = "";
  if (resizing.value) {
    classes = classes + "resizing";
  }
  return classes;
});

const offsetX = ref(0);
const offsetY = ref(0);
const dragging = ref(false);
const container = ref(null);
const image = ref(null);

const resizing = ref(false);

const showResize = (event) => {
  resizing.value = true;
};

const startDrag = (event) => {
  dragging.value = true;
  const rect = image.value.getBoundingClientRect();
  offsetX.value = event.clientX - rect.left;
  offsetY.value = event.clientY - rect.top;
};

const handleDrag = (event) => {
  if (dragging.value) {
    const posX = event.clientX - offsetX.value;
    const posY = event.clientY - offsetY.value;
    image.value.style.left = posX + "px";
    image.value.style.top = posY + "px";
  }
};

const endDrag = (event) => {
  dragging.value = false;
};
const clickedOutside = () => {
  resizing.value = false;
};

watch(
  () => props.position_left,
  () => {
    const posX = props.position_left;
    image.value.style.left = posX + "px";
  }
);
watch(
  () => props.position_top,
  () => {
    const posY = props.position_top;
    image.value.style.top = posY + "px";
  }
);
</script>

<style scoped>
.drag-container {
  position: relative;
}

.drag-image {
  position: absolute;
  cursor: pointer;
  border: 1px solid transparent;
}
.resizing {
  border: 1px solid white;
}
</style>
