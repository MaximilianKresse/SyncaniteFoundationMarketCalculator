<template>
  <img
    :src="
      currentMarketValue >= itemValue
        ? 'assets/' + marketType + '-token.png'
        : 'assets/missing-token.png'
    "
    width="40"
    height="40"
    draggable="false"
    @click="currentMarketValue = itemValue"
  />
</template>

<script setup lang="ts">
defineProps<{
  itemValue: number;
  marketType: 'food' | 'syncanite' | 'goods';
}>();

const currentMarketValue = defineModel<number>({ required: true });
</script>
