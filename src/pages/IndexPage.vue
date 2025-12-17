<template>
  <q-page
    class="row items-center non-selectable column bg-grey-4"
    style="font-family: 'Spectral SC'"
  >
    <div class="column q-py-md">
      <div class="row justify-center">{{ t('moneyValue') }}</div>
      <div class="row justify-center q-gutter-xs">
        <q-radio v-model="moneyValue" :val="-3" label="-3" color="dark" />
        <q-radio v-model="moneyValue" :val="-2" label="-2" color="dark" />
        <q-radio v-model="moneyValue" :val="-1" label="-1" color="dark" />
        <q-radio v-model="moneyValue" :val="0" label="0" color="dark" />
        <q-radio v-model="moneyValue" :val="1" label="1" color="dark" />
        <q-radio v-model="moneyValue" :val="2" label="2" color="dark" />
        <q-radio v-model="moneyValue" :val="3" label="3" color="dark" />
      </div>
      <div class="row justify-center q-gutter-xs" v-if="mode === 'marketState'">
        <q-radio v-model="marketType" val="food" :label="t('food')" color="dark" />
        <q-radio v-model="marketType" val="syncanite" :label="t('syncanite')" color="dark" />
        <q-radio v-model="marketType" val="goods" :label="t('goods')" color="dark" />
      </div>
    </div>
    <template v-if="marketType !== undefined">
      <template v-if="mode === 'marketState'">
        <market-state :market-type="marketType" v-model="currentMarketState" />
        <div class="row q-gutter-sm">
          <q-btn class="col" color="dark" @click="mode = 'buy'">{{ t('buy') }}</q-btn>
          <q-btn class="col" color="dark" @click="mode = 'sell'">{{ t('sell') }}</q-btn>
        </div>
      </template>
      <template v-else-if="mode === 'buy'">
        <q-btn color="dark" @click="mode = 'marketState'">{{ t('returnToMarket') }}</q-btn>
        <q-input
          v-model.number="additionalBuyModifier"
          type="number"
          :label="t('additionalBuyModifier')"
          dense
          class="q-my-xs"
          style="width: 220px"
          clearable
        />
        <market-buy
          :market-type="marketType"
          :moneyValue="buyModifier"
          v-model="currentMarketState"
        />
      </template>
      <template v-else-if="mode === 'sell'">
        <q-btn color="dark" @click="mode = 'marketState'">{{ t('returnToMarket') }}</q-btn>
        <q-input
          v-model.number="additionalSellModifier"
          type="number"
          :label="t('additionalSellModifier')"
          dense
          class="q-my-xs"
          style="width: 220px"
          clearable
        />
        <market-sell
          :market-type="marketType"
          :moneyValue="sellModifier"
          v-model="currentMarketState"
        />
      </template>
    </template>
  </q-page>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { useI18n } from 'vue-i18n';
import MarketState from '../components/MarketState.vue';
import MarketBuy from '../components/MarketBuy.vue';
import MarketSell from '../components/MarketSell.vue';

const { t } = useI18n();
const moneyValue = ref(0);
const marketType = ref<'food' | 'syncanite' | 'goods'>();
const fullMarketState = ref({
  food: 12,
  syncanite: 12,
  goods: 12,
});
const currentMarketState = computed<number>({
  get() {
    if (marketType.value === undefined) {
      return -1;
    }
    return fullMarketState.value[marketType.value];
  },
  set(value) {
    if (marketType.value === undefined || fullMarketState.value[marketType.value] === undefined) {
      return;
    }
    fullMarketState.value[marketType.value] = value;
  },
});
const mode = ref<'marketState' | 'buy' | 'sell'>('marketState');

const additionalBuyModifier = ref<number>();
const additionalSellModifier = ref<number>();

const buyModifier = computed(() => {
  if (typeof additionalBuyModifier.value !== 'number') {
    return moneyValue.value;
  }
  return moneyValue.value + additionalBuyModifier.value;
});
const sellModifier = computed(() => {
  if (typeof additionalSellModifier.value !== 'number') {
    return moneyValue.value;
  }
  return moneyValue.value + additionalSellModifier.value;
});
</script>
