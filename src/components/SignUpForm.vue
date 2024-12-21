<script setup lang="ts">
import { ref } from "vue";
import InputText from "./InputText.vue";
const userId = ref("");
const name = ref("");
const organization = ref("");

const handleSubmit = () => {
  alert(`User ID: ${userId.value}
Name: ${name.value}
Organization: ${organization.value}`);
};

// Validation rules
const userIdRules = [(value: string) => /^[a-zA-Z0-9]+$/.test(value), (value: string) => value.trim() !== ''];
const nameRules = [(value: string) => !/[\\/:*?"<>|]/.test(value), (value: string) => value.trim() !== ''];
const organizationRules = [(value: string) => !/[\\/:*?"<>|]/.test(value)];

const idValidationMessage = 'UserId에는 알파벳과 숫자만 허용되며 반드시 작성해야 합니다.';
const nameValidationMessage = 'Name에는 \\ / : * ? " < > | 특수문자를 사용할 수 없으며 반드시 작성해야 합니다.';
const organizationValidationMessage = 'Organization에는 \\ / : * ? " < > | 특수문자를 사용할 수 없습니다.';
</script>

<template>
  <form class="flex flex-col gap-4 py-4" @submit.prevent="handleSubmit">
    <InputText label="User ID" v-model="userId" :validationRules="userIdRules" :validationMessage="idValidationMessage" />
    <InputText label="Name" v-model="name" :validationRules="nameRules" :validationMessage="nameValidationMessage" />
    <InputText label="Organization" v-model="organization" :validationRules="organizationRules" :validationMessage="organizationValidationMessage" />
    <button class="bg-blue-500 text-white px-4 py-2 rounded-md">Submit</button>
  </form>
</template>
