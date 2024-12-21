<script setup lang="ts">
type InputTextProps = {
  label: string;
};
const model = defineModel<string>();
defineProps<InputTextProps>();

const clearInput = () => {
  model.value = '';
};
</script>

<template>
  <label class="flex flex-col gap-4 relative">
    {{ label }}
    <div class="relative w-full">
      <input
        class="w-full border-gray-400 border rounded-md h-10 px-4 pr-10 hover:border-blue-400 invalid:border-red-500 focus:border-green-400 focus:outline-none"
        type="text"
        autocomplete="off"
        v-model="model"
      />
      <!-- Clear button -->
      <button
        v-if="model"
        @click="clearInput"
        type="button"
        class="absolute right-2 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600 focus:outline-none"
      >
        ✕
      </button>
    </div>
  </label>
</template>
