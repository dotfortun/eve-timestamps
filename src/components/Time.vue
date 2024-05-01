<script setup lang="ts">
import { DateTime, Zone } from "luxon";
import { useClipboard } from "@vueuse/core";

const { title, time, tz, counter } = defineProps<{
  title?: string;
  time: DateTime;
  tz?: string | Zone;
  counter?: number;
}>();

const { text, copy, copied, isSupported } = useClipboard();
</script>

<template>
  <div class="card" :data-counter="counter">
    <h3>{{ title ? title : time.setZone(tz).toFormat("ZZZZZ") }}</h3>
    <span>{{
      time.setZone(tz).toLocaleString(DateTime.DATETIME_FULL_WITH_SECONDS)
    }}</span>
    <span
      class="copy"
      @click="
        copy(
          time.setZone(tz).toLocaleString(DateTime.DATETIME_FULL_WITH_SECONDS)
        )
      "
    >
      Copy time
      <i class="fa-regular fa-clone"></i>
    </span>
  </div>
</template>

<style scoped>
.card {
  @apply border-slate-500 border-solid border-2 rounded-md py-3 px-6 min-w-max;
  @apply flex flex-1 flex-row justify-between items-baseline;
  @apply lg:flex-col;
}

.copy {
  cursor: pointer;
}
</style>
