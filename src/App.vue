<template>
  <div class="page-wrapper">
    <header>
      <h1>Random Number Generator</h1>
      <AppButton title="Settings" @click="openModal">
        <img width="22" height="22" src="./assets/icons/IconSettings.svg" />
      </AppButton>
    </header>
    <SettingsModal v-model:array="ranges" ref="refModal" />

    <main>
      <template v-if="possibleValues.length">
        <div class="digits-wrapper">
          <template v-for="(digit, idx) in displayItems" :key="idx">
            <Transition name="slides" mode="out-in">
              <div class="digit" :key="digit">
                {{ digit }}
              </div>
            </Transition>
          </template>
        </div>
      </template>
      <div v-else class="digits-wrapper">No numbers available</div>

      <AppButton class="generate-button" @click="generateNumber">Generate</AppButton>
    </main>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import AppButton from './components/UI-kit/AppButton.vue'
import SettingsModal from './components/SettingsModal.vue'
// import { waitFor } from './helpers/waitFor'

const refModal = ref()

const ranges = ref<[number, number][]>([[0, 101]])
const chosenNumbers = ref<number[]>([])

const currentNumber = ref(0)

const possibleValues = computed<number[]>(() => {
  return ranges.value
    .map(([start, end]) =>
      (Array.from({ length: end - start }).fill(start) as number[]).map((val, idx) => val + idx),
    )
    .flat()
    .filter((val) => !chosenNumbers.value.includes(val))
})

const maxPossibleValue = computed(() => Math.max(...possibleValues.value) ?? '')

const getRandomApproxRange = () => {
  return Math.floor(Math.random() * maxPossibleValue.value)
}

const generateNumber = async () => {
  const randomIndex = Math.floor(Math.random() * possibleValues.value.length)
  const target = possibleValues.value[randomIndex]
  // currentNumber.value = target - getRandomApproxRange()
  // await waitFor(500)
  // currentNumber.value = target - getRandomApproxRange()
  // await waitFor(500)
  currentNumber.value = target
  chosenNumbers.value.push(target)
}

const displayItems = computed(() =>
  (Array.from({ length: maxPossibleValue.value.toString().length }).fill(0) as number[]).map(
    (_, idx) =>
      currentNumber.value
        .toString()
        .padStart(maxPossibleValue.value.toString().length, '0')
        .charAt(idx),
  ),
)

const openModal = () => {
  refModal.value?.open()
}
</script>

<style scoped>
.page-wrapper {
  display: flex;
  width: 100%;
  height: 100dvh;
  flex-direction: column;
  color: var(--color-text);
  font:
    400 16px/24px 'Inter',
    sans-serif;
}

header {
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: 600;
  font-size: 20px;
  line-height: 24px;
}

main {
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.digits-wrapper {
  display: flex;
  gap: 0.5rem;
  font-size: 2rem;
  font-weight: bold;
  margin-bottom: 3rem;
}

.digit {
  border: 1px solid black;
  border-radius: 6px;
  padding: 0.4rem;
}

.slides-enter-active,
.slides-leave-active {
  transition: all 0.5s ease;
  transform: translateY(0px);
}

.slides-enter-from {
  opacity: 0;
  transform: translateY(-50px);
}
.slides-leave-to {
  opacity: 0;
  transform: translateY(50px);
}
</style>
