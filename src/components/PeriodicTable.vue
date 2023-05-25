<script setup lang="ts">
import { computed, ref } from "vue";
import ElementCell from "./ElementCell.vue";
import tableElements from "../data/table-data";

const highlightedElement = ref<string>();
const invertTable = ref<boolean>(false);
const toggleTableInversion = () => {
  invertTable.value = !invertTable.value;
};
const searchInput = ref<string>("");

const inverted = ref<Boolean>(false);

const toggleHighlight = (block: string) => {
  highlightedElement.value === block
    ? (highlightedElement.value = undefined)
    : (highlightedElement.value = block);
};
const tableInvertion = computed(() => {
  const updatedElements = inverted.value
    ? tableElements.map((element) => ({
        ...element,
        row: 10 - element.row,
      }))
    : tableElements;

  return updatedElements;
});
</script>

<template>
  <input type="text" v-model="searchInput" />
  <button class="invert-css" @click="toggleTableInversion">
    Invert Table with css
  </button>
  <button class="invert-js" @click="inverted = !inverted">
    Invert table with js
  </button>
  <div class="periodic-table" :class="{ inverted: invertTable }">
    <ElementCell
      class="element-cell"
      v-for="element in tableInvertion"
      :key="element.atomicNumber"
      :atomic-number="element.atomicNumber"
      :name="element.name"
      :symbol="element.symbol"
      :block="element.block"
      :is-highlighted="element.block === highlightedElement"
      :is-match="
        searchInput.length ? element.name.includes(searchInput) : false
      "
      @highlight="toggleHighlight(element.block)"
      :style="{ gridColumn: element.column, gridRow: element.row }"
    />
  </div>
</template>

<style scoped lang="scss">
.periodic-table {
  display: grid;
  grid-template-columns: repeat(18, 1fr);
  grid-gap: 10px;
  width: 100%;
  transform: scaleY(1);
}

.periodic-table.inverted {
  transform: scaleY(-1);
}

.element-cell {
  transform: scaleY(1);
}

.periodic-table.inverted .element-cell {
  transform: scaleY(-1);
}

.invert {
  &-css {
    position: fixed;
    bottom: 30px;
    right: 30px;
  }
  &-js {
    position: fixed;
    bottom: 30px;
    right: 200px;
  }
}
input {
  border: 1px solid #ccc;
}

@media (max-width: 1024px) {
  .periodic-table {
    grid-gap: 5px;
  }

  .element-cell {
    font-size: 80%;
  }
}

@media (max-width: 768px) {
  .periodic-table {
    grid-template-columns: repeat(9, 1fr);
    grid-gap: 5px;
  }

  .element-cell {
    font-size: 70%;
  }
}
</style>
