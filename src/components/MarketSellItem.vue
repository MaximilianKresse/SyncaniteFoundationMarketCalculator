<template>
  <img
    v-if="currentMarketState >= itemValue"
    :src="imageUrl"
    width="40"
    height="40"
    draggable="false"
  />
  <img
    v-else
    :src="imageUrl"
    width="40"
    height="40"
    draggable="false"
    @click="newMarketState = itemValue"
    :class="{ isSelected: newMarketState !== null && newMarketState >= itemValue }"
  />
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
  currentMarketState: number;
}>();

const newMarketState = defineModel<null | number>({ required: true });

const imageUrl = computed(() => {
  if (props.currentMarketState < props.itemValue) {
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
</script>

<style scoped>
.isSelected {
  border: 3px solid red;
}
</style>
