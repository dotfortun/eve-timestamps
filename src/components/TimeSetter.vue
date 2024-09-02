<script setup lang="ts">
import { DateTime } from "luxon";
import { ref } from "vue";
import { useBrowserLocation } from "@vueuse/core";

const { modelValue } = defineProps<{
  modelValue: DateTime | null;
}>();
const location = useBrowserLocation();

const emit = defineEmits<{
  (e: "update:modelValue", value: string | null): void;
  (e: "reset"): void;
}>();

const offset = ref({
  day: 0,
  hour: 0,
  minute: 0,
});

const setTime = ref("");
</script>

<template>
  <div class="offset">
    <span>In:</span>
    <label
      ><input
        @focus="$event.target?.select()"
        type="number"
        v-model="offset.day"
      />
      days</label
    >
    <label
      ><input
        @focus="$event.target?.select()"
        type="number"
        v-model="offset.hour"
      />
      hours</label
    >
    <label
      ><input
        @focus="$event.target?.select()"
        type="number"
        v-model="offset.minute"
      />
      minutes</label
    >
    <button
      class="set"
      @click="emit('update:modelValue', DateTime.now().plus(offset).toISO())"
    >
      Set
    </button>
  </div>

  <div class="picker">
    <label>
      Set Time:
      <input
        type="datetime-local"
        :value="setTime"
        @input="setTime = ($event.target as HTMLInputElement).value"
      />
    </label>
    <button
      class="set"
      @click="emit('update:modelValue', setTime)"
      :disabled="!setTime"
    >
      Set
    </button>
  </div>

  <button
    class="reset"
    @click="
      emit('reset');
      offset = {
        day: 0,
        hour: 0,
        minute: 0,
      };
      setTime = '';
    "
  >
    Reset
  </button>
</template>

<style scoped>
div.offset,
div.picker {
  @apply my-2;
  @apply flex flex-1 flex-col lg:flex-row items-baseline gap-1;
}

button.set {
  @apply ml-2 my-2;
}

input {
  @apply bg-sky-900 px-2 py-1 rounded-md mb-2;
}

input[type="number"] {
  max-width: 3.5rem;
}
</style>
@focus="$event.target.select()"
