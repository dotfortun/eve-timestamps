<script setup>
import { ref, watch } from "vue";
import { useInterval, useUrlSearchParams } from "@vueuse/core";
import { DateTime, Zone } from "luxon";

import Time from "./components/Time.vue";
import DiscordTS from "./components/DiscordTS.vue";
import TimeSetter from "./components/TimeSetter.vue";

const timezones = ref([
  { title: "GMT", tz: "utc" },
  { title: "US Eastern / New York", tz: "America/New_York" },
  { title: "US Central / Chicago", tz: "America/Chicago" },
  { title: "US Mountain / Denver", tz: "America/Boise" },
  { title: "US Pacific / Los Angeles", tz: "America/Los_Angeles" },
  { title: "EU Western / London", tz: "Europe/London" },
  { title: "EU Central / Germany", tz: "Europe/Berlin" },
  { title: "EU Eastern / Türkiye", tz: "Europe/Istanbul" },
  { title: "Russia / Moscow", tz: "Europe/Moscow" },
  { title: "China / Shanghai", tz: "Asia/Shanghai" },
  { title: "Australia / Sydney", tz: "Australia/Sydney" },
]);

const { t } = useUrlSearchParams("history");

const counter = useInterval(1000);
const time = ref(DateTime.now());
const timeModel = defineModel();
const diceroll = ref(Math.random());

watch(counter, () => {
  if (!timeModel.value) {
    time.value = DateTime.now();
  } else {
    time.value = DateTime.fromISO(timeModel.value);
  }
});

console.log(t);

if (t !== undefined) {
  console.log(DateTime.fromSeconds(parseInt(t)).toISO());
  timeModel.value = DateTime.fromSeconds(parseInt(t)).toISO();
}
</script>

<template>
  <div class="header">
    <h1>Once More With Timestamps</h1>
  </div>
  <div class="share">
    <DiscordTS :time="time" :counter="counter" />
  </div>
  <div class="controls">
    <TimeSetter v-model="timeModel" @reset="timeModel = null" />
  </div>
  <div class="timezones">
    <Time title="System Time" :time="time" tz="system" :counter="counter" />
    <template v-for="zone of timezones">
      <Time :title="zone.title" :time="time" :tz="zone.tz" :counter="counter" />
    </template>
  </div>
  <footer>
    <div>
      <small>
        <ul>
          <li class="list-none">
            Created by <a href="https://github.com/dotfortun">dotfortun.</a>
          </li>
          <li class="list-none">
            <template v-if="diceroll < 0.05">
              Yes, this is basically just Nakamura Labs' Time tool but with
              Discord timestamps.
            </template>
            <template v-else>
              Donations to Peter Dostoevsky in Eve Online will be turned into
              lossmails.
            </template>
          </li>
          <li class="list-none">
            <a
              href="https://www.eveonline.com/signup?invc=5d52b821-87c0-4c81-bd0a-2cd88e9001b6"
              target="_blank"
              rel="noopener noreferrer"
            >
              Try Eve Online
            </a>
            or
            <a
              href="https://www.buymeacoffee.com/shanebelldev"
              target="_blank"
              rel="noopener noreferrer"
              >buy me a coffee</a
            >
          </li>
        </ul>
      </small>
    </div>
    <div>
      <p>
        <a href="https://github.com/dotfortun/eve-timestamps/issues">
          Found a bug? Tell us here!
        </a>
      </p>
    </div>
  </footer>
</template>

<style scoped>
div {
  @apply prose prose-invert max-w-none;
}

div.header {
  @apply container mx-auto mb-3 flex justify-center lg:justify-start;
}

div.timezones {
  @apply container mx-auto grid grid-cols-1 lg:grid-cols-3 gap-2 mt-2 mb-3;
}

div.controls {
  @apply container mx-auto mb-3 flex flex-grow-0 flex-shrink flex-col justify-around;
  @apply items-center xl:flex-row gap-1 flex-wrap;
}

div.share {
  @apply container mx-auto mb-3 flex justify-around xl:justify-start;
}

footer {
  @apply container mx-auto mb-3;
  @apply flex flex-row content-center justify-between p-4 text-slate-400;
  @apply max-lg:flex-col max-lg:items-center max-lg:text-center;
}

footer a {
  @apply text-cyan-400 decoration-solid;
}

footer ul {
  @apply pl-0;
}
</style>
