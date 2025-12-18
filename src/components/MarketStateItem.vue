<template>
  <img :src="imageUrl" width="40" height="40" draggable="false" @click="onClick" />
</template>

<script setup lang="ts">
import { computed } from 'vue';
import missingToken from '../assets/missing-token.png?inline';
import foodToken from '../assets/food-token.png?inline';
import syncaniteToken from '../assets/syncanite-token.png?inline';
import goodsToken from '../assets/goods-token.png?inline';

const props = defineProps<{
  itemValue: number;
  marketType: 'food' | 'syncanite' | 'goods';
}>();

const currentMarketValue = defineModel<number>({ required: true });

const imageUrl = computed(() => {
  if (currentMarketValue.value < props.itemValue) {
    return missingToken;
  }
  if (props.marketType === 'food') {
    return foodToken;
  }
  if (props.marketType === 'syncanite') {
    return syncaniteToken;
  }
  if (props.marketType === 'goods') {
    return goodsToken;
  }
  throw new Error('Unknown market type');
});

function onClick() {
  if (props.itemValue === 1 && currentMarketValue.value === 1) {
    currentMarketValue.value = 0;
    return;
  }
  currentMarketValue.value = props.itemValue;
}
</script>
