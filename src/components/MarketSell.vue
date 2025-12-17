<template>
  <div class="row justify-center">
    <div class="column q-mb-xs">
      <div class="row q-gutter-xs q-mb-xs" v-for="n in 10" :key="n">
        <market-sell-item
          :currentMarketState="currentMarketState"
          :item-value="n * 2 - 1"
          :marketType="marketType"
          v-model="newMarketState"
        />
        <market-sell-item
          :currentMarketState="currentMarketState"
          :item-value="n * 2"
          :marketType="marketType"
          v-model="newMarketState"
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
  </div>
  <p class="text-center text-h4" v-if="sum >= 0">{{ t('get', { sum }) }}</p>
  <p class="text-center text-h4" v-else>{{ t('pay', { sum: sum * -1 }) }}</p>
  <q-btn
    :label="t('updateMarket')"
    color="dark"
    @click="if (newMarketState !== null) currentMarketState = newMarketState;"
  />
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { useI18n } from 'vue-i18n';
import MarketSellItem from './MarketSellItem.vue';
import { backgroundColors, marketInfos } from '../marketInfos';

const props = defineProps<{
  marketType: 'food' | 'syncanite' | 'goods';
  moneyValue: number;
}>();
const currentMarketState = defineModel<number>({ required: true });

const { t } = useI18n();

const newMarketState = ref(null);

const items = computed(() => {
  if (currentMarketState.value === 0 || newMarketState.value === null) {
    return [];
  }
  return marketInfos[props.marketType].slice(currentMarketState.value, newMarketState.value);
});
const calculatedItems = computed(() => {
  return items.value.map((item) => item + props.moneyValue);
});
const sum = computed(() => {
  return calculatedItems.value.reduce((currentSum, value) => currentSum + value, 0);
});
</script>
