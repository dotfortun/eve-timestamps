<script setup lang="ts">
import { DateTime } from "luxon";
import { ref } from "vue";

const { modelValue } = defineProps<{
  modelValue: DateTime | null;
}>();

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
    In:
    <label><input type="number" v-model="offset.day" /> days </label>
    <label><input type="number" v-model="offset.hour" /> hours </label>
    <label><input type="number" v-model="offset.minute" /> minutes </label>
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
    <button class="set" @click="emit('update:modelValue', setTime)">Set</button>
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
    "
  >
    Reset
  </button>
</template>

<style scoped>
div.offset,
div.picker {
  @apply my-2;
}

button.reset {
  @apply bg-red-700 p-2 rounded-md ml-3 h-min;
}

button.set {
  @apply ml-2;
}

input {
  @apply bg-sky-900 px-2 py-1 rounded-md;
}

input[type="number"] {
  max-width: 3rem;
}
</style>
