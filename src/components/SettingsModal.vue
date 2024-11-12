<template>
  <Teleport to="body">
    <dialog ref="refDialog">
      <form>
        <div class="inputs-wrapper">
          <div v-for="(val, idx) in array" :key="idx" class="input-line">
            <InputNumber label="Min" v-model="array[idx][0]" />
            <InputNumber label="Max" v-model="array[idx][1]" />
          </div>

          <button class="add-button" title="Add" @click.prevent="addRange">+</button>
        </div>

        <div><AppButton type="submit" @click.prevent="closeForm">Ok</AppButton></div>
      </form>
    </dialog>
  </Teleport>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import AppButton from './UI-kit/AppButton.vue'
import InputNumber from './UI-kit/InputNumber.vue'

const refDialog = ref<HTMLDialogElement>()

const array = defineModel<[number, number][]>('array', { required: true })

const addRange = () => {
  array.value.push([0, 0])
}

const closeForm = () => {
  refDialog.value?.close()
}

defineExpose({
  open: () => refDialog.value?.showModal(),
})
</script>

<style scoped lang="css">
dialog::backdrop {
  opacity: 0.5;
  background-color: var(--color-text);
}

dialog {
  width: min(40dvh, 400px);
  border-radius: 0.5rem;
  border: 1px solid var(--color-shadow);
}

form {
  display: flex;
  flex-direction: column;
}

.inputs-wrapper {
  flex-grow: 1;
  margin-bottom: 3rem;
}

.input-line {
  display: flex;
  gap: 2rem;
  align-items: center;
  margin-bottom: 1rem;
}
</style>
