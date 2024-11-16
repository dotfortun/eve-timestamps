<script setup lang="ts">
import { ref, watch } from "vue";
import { DateTime, Zone } from "luxon";
import { useClipboard, useTimeAgo, useTimeout } from "@vueuse/core";

const { time, counter } = defineProps<{
  time: DateTime;
  counter?: number;
}>();

const { text, copy, copied, isSupported } = useClipboard();
const { ready, start, stop } = useTimeout(1500, { controls: true });
stop();

const timestamp = (time: DateTime, format: string = ":R") => {
  return `<t:${time.toUnixInteger()}${format}>`;
};

const formats = ref([
  {
    name: "Default",
    value: "",
  },
  {
    name: "Short Time",
    value: ":t",
  },
  {
    name: "Long Time",
    value: ":T",
  },
  {
    name: "Short Date",
    value: ":d",
  },
  {
    name: "Long Date",
    value: ":D",
  },
  {
    name: "Short Date/Time",
    value: ":f",
  },
  {
    name: "Long Date/Time",
    value: ":F",
  },
  {
    name: "Relative Time",
    value: ":R",
  },
]);
const selected = ref(formats.value[0]);
</script>

<template>
  <div>
    <select class="text-slate-900" v-model="selected" v-once>
      <template v-for="format of formats">
        <option :value="format">{{ format.name }}</option>
      </template>
    </select>
    <button
      class="set"
      @click="
        copy(timestamp(time, selected.value));
        start();
      "
      :data-counter="counter"
    >
      {{ ready ? "Copy Discord Tag" : "Tag Copied" }}
    </button>
    <button
      class="set ml-2"
      @click="
        copy(`https://time.dotlag.space/?t=${Math.floor(time.toSeconds())}`)
      "
    >
      Share Time
    </button>
  </div>
</template>

<style scoped>
select {
  @apply mr-2;
  @apply bg-sky-900 text-slate-200 px-2 py-1 rounded-md;
}
</style>
