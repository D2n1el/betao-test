<script lang="ts" setup>
import { computed } from "vue";

const props = defineProps<{
  name: string;
  symbol: string;
  atomicNumber: Number;
  block: string;
  isHighlighted: Boolean;
}>();

const getBackgroundColor = (block: string | undefined, highlighted = false) => {
  switch (block) {
    case "s":
      return highlighted ? "rgba(210, 77, 87, 0.5)" : "rgba(236, 100, 75, 0.7)";
    case "p":
      return highlighted
        ? "rgba(147, 250, 165, 0.5)"
        : "rgba(13, 180, 185, 0.7)";
    case "d":
      return highlighted ? "rgba(1, 1, 122, 0.5)" : "rgba(37, 41, 88, 0.7)";
    case "f":
      return highlighted
        ? "rgba(213, 184, 255, 0.5)"
        : "rgba(191, 85, 236, 0.7)";
  }
};

const backgroundColor = computed(() => getBackgroundColor(props.block));
const highlightedBackgroundColor = computed(() =>
  getBackgroundColor(props.block, true)
);

const emit = defineEmits(["highlight"]);
</script>

<template>
  <div
    @click="emit('highlight')"
    class="element-wrapper"
    :style="{
      'background-color':
        (props.isHighlighted ? highlightedBackgroundColor : backgroundColor) ||
        'transparent',
    }"
  >
    <span style="font-size: 12px">{{ props.atomicNumber }}</span>
    <span>{{ props.symbol }}</span>
    <span style="font-size: 10px">{{ props.name }}</span>
  </div>
</template>

<style lang="scss" scoped>
h2 {
  margin: 0;
  padding: 0;
}

.element-wrapper {
  border: 0.5px solid #ccc;
  padding: 10px 0px !important;
  padding: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}
</style>
