<script setup lang="ts">
import { DateTime, Zone } from "luxon";
import { useClipboard, useTimeout } from "@vueuse/core";

const { title, time, tz, counter } = defineProps<{
  title?: string;
  time: DateTime;
  tz?: string | Zone;
  counter?: number;
}>();

const { text, copy, copied, isSupported } = useClipboard();
const { ready, start, stop } = useTimeout(1500, { controls: true });
stop();
</script>

<template>
  <div class="card" :data-counter="counter">
    <h3>{{ title ? title : time.setZone(tz).toFormat("ZZZZZ") }}</h3>
    <span>{{
      time.setZone(tz).toLocaleString(DateTime.DATETIME_MED_WITH_SECONDS)
    }}</span>
    <span
      class="copy"
      @click="
        copy(
          time.setZone(tz).toLocaleString(DateTime.DATETIME_FULL_WITH_SECONDS)
        );
        start();
      "
    >
      {{ ready ? "Copy time" : "Copied" }}
      <i class="fa-regular fa-clone"></i>
    </span>
  </div>
</template>

<style scoped>
h3 {
  margin-top: 0.25rem !important;
}

.card {
  @apply border-slate-500 border-solid border-2 rounded-md py-3 px-6 min-w-max;
  @apply flex flex-1 flex-col justify-between items-baseline;
  @apply sm:flex-row lg:flex-col lg:flex-wrap;
}

.copy {
  cursor: pointer;
}
</style>
