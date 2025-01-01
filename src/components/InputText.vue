<script setup lang="ts">
import { ref, computed, watch } from 'vue';

type InputTextProps = {
  label: string;
  validationRules: Array<(value: string) => boolean>;
  validationMessage: string;
  modelValue: string;
};

const props = defineProps<InputTextProps>();
const model = ref(props.modelValue);
const errorMessage = ref('');
const isFocused = ref(false);

const emit = defineEmits<{
  (event: 'update:modelValue', value: string): void;
}>();

const isValid = computed(() => {
  console.log('validation check!');
  return props.validationRules.every((rule) => rule(model.value));
});

watch(model, (newValue) => {
  emit('update:modelValue', newValue);
  errorMessage.value = isValid.value ? '' : props.validationMessage;
});

const clearInput = () => {
  model.value = '';
  emit('update:modelValue', '');
};
</script>

<template>
  <label class="flex flex-col gap-4 relative">
    {{ label }}
    <div class="relative w-full">
      <input
        class="w-full border rounded-md h-10 px-4 pr-10 hover:border-blue-400 focus:outline-none"
        :class="{
          'border-gray-400': !errorMessage && !isFocused,
          'border-red-500': errorMessage && !isFocused,
          'border-red-700': errorMessage && isFocused,
          'focus:border-green-400': !errorMessage && isFocused,
        }"
        type="text"
        autocomplete="off"
        v-model="model"
        @focus="isFocused = true"
        @blur="isFocused = false"
      />
      <!-- Clear button -->
      <button
        v-if="model && model.length > 0"
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
