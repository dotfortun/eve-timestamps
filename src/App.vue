<script setup>
import { ref, watch } from "vue";
import { useInterval } from "@vueuse/core";
import { DateTime, Zone } from "luxon";

import Time from "./components/Time.vue";

const timezones = ref(["utc", "America/New_York", "America/Los_Angeles"]);

const counter = useInterval(1000);
const time = ref(DateTime.now());

watch(counter, () => {
  time.value = DateTime.now();
});
</script>

<template>
  <div>
    <h1>Once More With Timestamps</h1>
    <div class="timezones">
      <Time title="System Time" :time="time" tz="system" :counter="counter" />
      <template v-for="tz of timezones">
        <Time :time="time" :tz="tz" :counter="counter" />
      </template>
    </div>
  </div>
</template>

<style scoped>
div {
  @apply prose prose-invert max-w-none;
}

div.timezones {
  @apply grid grid-cols-1 lg:grid-cols-4 gap-2;
}
</style>
