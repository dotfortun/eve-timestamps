<script setup>
import { DateTime } from "luxon";
import { ref } from "vue";

const { modelValue } = defineProps(["modelValue"]);
const emit = defineEmits(["update:modelValue", "reset"]);
</script>

<template>
  <div>
    <input
      type="datetime-local"
      :value="modelValue"
      @input="emit('update:modelValue', $event.target.value)"
    />
    <button @click="emit('reset')">Reset</button>
  </div>
</template>

<style scoped>
div {
  @apply flex text-slate-800;
}

button {
  @apply bg-red-700 p-2 rounded-md ml-3;
}
</style>
