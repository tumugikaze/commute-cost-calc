<template>
  <v-app>
    <v-main>
      <CommuteCostCalc />
      <v-btn
        class="m-2"
        icon="mdi-theme-light-dark"
        location="top right"
        position="absolute"
        @click="$vuetify.theme.cycle()"
      />
    </v-main>
  </v-app>
</template>

<script lang="ts" setup>
// biome-ignore lint/correctness/noUnusedImports: used in template
import CommuteCostCalc from '@/components/CommuteCostCalc.vue'
</script>
