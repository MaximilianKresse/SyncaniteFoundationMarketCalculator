<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated class="bg-black text-white non-selectable" height-hint="98">
      <q-toolbar style="font-family: 'Spectral SC'; color: #e1b760">
        <img
          src="~/assets/syncanite-foundation.png"
          alt="Syncanite Foundation"
          style="height: 40px"
        />
        <q-toolbar-title style="font-weight: 600; font-size: 15px">
          Market Calculator
        </q-toolbar-title>
        <q-select
          v-model="locale"
          :options="localeOptions"
          dark
          dense
          borderless
          emit-value
          map-options
          options-dense
          style="font-size: 11px"
        />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { useI18n } from 'vue-i18n';

const { locale } = useI18n({ useScope: 'global' });
const localeOptions = [
  { value: 'de-DE', label: 'Deutsch' },
  { value: 'en-US', label: 'English' },
];
</script>
