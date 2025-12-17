<template>
  <div class="column q-mb-xs">
    <div class="row q-gutter-xs q-mb-xs" v-for="n in 10" :key="n">
      <market-state-item
        v-model="currentMarketState"
        :item-value="n * 2 - 1"
        :marketType="marketType"
      />
      <market-state-item
        v-model="currentMarketState"
        :item-value="n * 2"
        :marketType="marketType"
      />
      <span
        class="text-center"
        style="width: 40px; padding-top: 9px; color: white"
        :style="{ backgroundColor: backgroundColors[marketType] }"
      >
        {{ marketInfos[marketType][n * 2 - 2] }}
      </span>
    </div>
  </div>
</template>

<script setup lang="ts">
import MarketStateItem from '../components/MarketStateItem.vue';
import { backgroundColors, marketInfos } from '../marketInfos';

defineProps<{
  marketType: 'food' | 'syncanite' | 'goods';
}>();

const currentMarketState = defineModel<number>({ required: true });
</script>
