<script setup lang="ts">
import { ref, computed, watch } from 'vue';

type InputTextProps = {
  label: string;
  validationRules: Array<(value: string) => boolean>;
  validationMessage: string;
};

const props = defineProps<InputTextProps>();
const model = defineModel<string>();
const errorMessage = ref('');

// 유효성 검사 로직: rules을 따라 검사 후 오류 메시지 반환
const isValid = computed(() => {
  return props.validationRules.every((rule) => rule(model.value));
});

// 유효하지 않으면 errorMessage 설정
watch(isValid, (valid) => {
  if (valid) {
    errorMessage.value = '';
  } else {
    errorMessage.value = props.validationMessage;
  }
});

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
    <!-- Validation message -->
    <span v-if="errorMessage" class="text-red-500 text-sm">{{ errorMessage }}</span>
  </label>
</template>
