<script setup lang="ts">
import { ref, computed, watch } from 'vue';
import InputText from './InputText.vue';

const userId = ref('');
const name = ref('');
const organization = ref('');

// Validation rules
const userIdRules = [(value: string) => /^[a-zA-Z0-9]+$/.test(value), (value: string) => value.trim() !== '', (value: string) => !/[가-힣]/.test(value)];
const nameRules = [(value: string) => !/[\\/:*?"<>|]/.test(value), (value: string) => value.trim() !== ''];
const organizationRules = [(value: string) => !/[\\/:*?"<>|]/.test(value)];

// Validation Messages
const idValidationMessage = 'UserId에는 알파벳과 숫자만 허용되며 반드시 작성해야 합니다.';
const nameValidationMessage = 'Name에는 \\ / : * ? " < > | 특수문자를 사용할 수 없으며 반드시 작성해야 합니다.';
const organizationValidationMessage = 'Organization에는 \\ / : * ? " < > | 특수문자를 사용할 수 없습니다.';

const errorMessages = ref({
  userId: '',
  name: '',
  organization: '',
});

const isUserIdValid = computed(() => userIdRules.every((rule) => rule(userId.value)));
const isNameValid = computed(() => nameRules.every((rule) => rule(name.value)));
const isOrganizationValid = computed(() => organizationRules.every((rule) => rule(organization.value)));

watch([userId, name, organization], () => {
  errorMessages.value.userId = isUserIdValid.value ? '' : idValidationMessage;
  errorMessages.value.name = isNameValid.value ? '' : nameValidationMessage;
  errorMessages.value.organization = isOrganizationValid.value ? '' : organizationValidationMessage;
});

// Form validation (all fields must be valid to submit)
const isFormValid = computed(() => isUserIdValid.value && isNameValid.value && isOrganizationValid.value);

const handleSubmit = () => {
  if (isFormValid.value) {
    alert(`User ID: ${userId.value}
    Name: ${name.value}
    Organization: ${organization.value}`);
  } else {
    alert('Please fill in the form correctly.');
  }
};
</script>

<template>
  <form class="flex flex-col gap-4 py-4" @submit.prevent="handleSubmit">
    <InputText label="User ID" v-model="userId" :validationRules="userIdRules" :validationMessage="idValidationMessage" />
    <InputText label="Name" v-model="name" :validationRules="nameRules" :validationMessage="nameValidationMessage" />
    <InputText label="Organization" v-model="organization" :validationRules="organizationRules" :validationMessage="organizationValidationMessage" />
    <button
      type="submit"
      class="text-white px-4 py-2 rounded-md"
      :class="{
        'bg-blue-500': isFormValid,
        'bg-gray-400 cursor-not-allowed': !isFormValid,
      }"
      :disabled="!isFormValid"
    >
      Submit
    </button>
  </form>
</template>
