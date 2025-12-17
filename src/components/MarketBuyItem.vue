<template>
  <img
    v-if="currentMarketState >= itemValue"
    :src="'assets/' + marketType + '-token.png'"
    width="40"
    height="40"
    draggable="false"
    @click="newMarketState = itemValue"
    :class="{ isSelected: newMarketState !== null && newMarketState <= itemValue }"
  />
  <img v-else src="assets/missing-token.png" width="40" height="40" draggable="false" />
</template>

<script setup lang="ts">
defineProps<{
  itemValue: number;
  marketType: 'food' | 'syncanite' | 'goods';
  currentMarketState: number;
}>();

const newMarketState = defineModel<null | number>({ required: true });
</script>

<style scoped>
.isSelected {
  border: 3px solid red;
}
</style>
