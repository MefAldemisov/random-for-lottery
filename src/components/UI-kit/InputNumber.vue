<template>
  <div class="input-number">
    <label :for="`input-${id}`">{{ props.label }}:</label>
    <input :id="`input-${id}`" type="number" v-model.number="value" />
  </div>
</template>

<script setup lang="ts">
import { generateId } from '@/helpers/generateId'

const id = generateId()

const value = defineModel()
const props = defineProps<{
  label: string
}>()
</script>

<style scoped>
.input-number {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.input-number > label {
  font-size: 0.8rem;
}

.input-number > input {
  outline: none;
}
</style>
