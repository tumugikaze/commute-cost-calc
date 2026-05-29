<template>
  <v-container class="max-w-xl mx-auto py-8 px-4">
    <h1 class="text-h4 font-bold mb-6">通勤費計算</h1>

    <v-card class="elevation-1 mb-4 rounded-lg">
      <v-card-text class="pa-5">
        <v-text-field
          v-model="oneWayCost"
          hide-details
          label="片道電車賃"
          prefix="¥"
          type="number"
          min="0"
          variant="outlined"
        />
      </v-card-text>
    </v-card>

    <v-card class="elevation-1 mb-4 rounded-lg">
      <v-card-title class="text-body-1 pa-5 pb-2 text-medium-emphasis">
        利用日を選択
      </v-card-title>

      <v-card-text class="px-0 pt-0 flex justify-center">
        <v-date-picker
          v-model="selectedDates"
          multiple
        >
          <template #title></template>
          <template #header></template>
          <template #controls="{disabled, nextMonth, prevMonth, monthYearText}">
            <v-btn
              :disabled="disabled.includes('prev-month')"
              color="primary"
              icon="$prev"
              @click="prevMonth"
            ></v-btn>
            <v-spacer />
            <div class="text-center">
              <div class="text-body-small my-n1 text-primary">
                {{ monthYearText.split(' ')[1] }}
              </div>
              <div class="text-body-large">{{ monthYearText.split(' ')[0] }}</div>
            </div>
            <v-spacer />
            <v-btn
              :disabled="disabled.includes('next-month')"
              color="primary"
              icon="$next"
              @click="nextMonth"
            ></v-btn>
          </template>
        </v-date-picker>
      </v-card-text>
    </v-card>

    <v-card class="elevation-1 rounded-lg">
      <v-card-text class="pa-5">
        <div class="flex justify-between items-center mb-2">
          <span class="text-body-1 text-medium-emphasis">日数</span>
          <span class="text-body-1 font-medium">{{ selectedDates.length }} 日</span>
        </div>

        <div class="flex justify-between items-center">
          <span class="text-h6">合計</span>
          <span class="text-h5 font-bold text-primary">¥  {{ totalCost }}</span>
        </div>

          <template v-if="formattedDates">
            <v-divider class="my-4" />
            <v-text-field
              v-model="formattedDates"
              hide-details
              label="利用日一覧"
              type="string"
              variant="outlined"
              append-inner-icon="mdi-content-copy"
              @click:append-inner="copyText"
            >
              <template v-slot:append-inner>
                <v-btn v-if="!isCopied" variant="text" icon="mdi-content-copy" @click="copyText"></v-btn>
                <v-icon v-else icon="mdi-check-bold" />
              </template>
            </v-text-field>
          </template>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script lang="ts" setup>
import { computed, onUnmounted, ref, watch } from "vue"

const oneWayCost = ref<number | null>(Number(localStorage.getItem('oneWayCost') ?? 0))
const selectedDates = ref<(string | Date)[]>([])
const isCopied = ref<boolean>(false)

const toDate = (d: string | Date) => (d instanceof Date ? d : new Date(d))

let timer: ReturnType<typeof setTimeout> | undefined

// biome-ignore lint/correctness/noUnusedVariables: used in template
const totalCost = computed(
    () => {
      if (oneWayCost.value === null) return 0
      return oneWayCost.value * 2 * selectedDates.value.length
    },
)

watch(oneWayCost, (val) => {
  localStorage.setItem('oneWayCost', String(val))
})

const formattedDates = computed(() =>
    [...selectedDates.value]
        .sort((a, b) => toDate(a).getTime() - toDate(b).getTime())
        .map((d) => {
            const date = toDate(d)
            return `${date.getMonth() + 1}/${date.getDate()}`
        })
        .join(", "),
)

// biome-ignore lint/correctness/noUnusedVariables: used in template
const copyText = () => {
  navigator.clipboard.writeText(formattedDates.value)
    .then(() => {
      isCopied.value = true
      timer = setTimeout(() => {
        isCopied.value = false
      }, 1000)
    })
}

onUnmounted(() => clearTimeout(timer))
</script>
