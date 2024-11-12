<template>
  <button class="app-button">
    <slot></slot>
  </button>
</template>

<style lang="css" scoped>
.app-button {
  cursor: pointer;
  padding: 0.5rem 1rem;
  font-size: 1.2rem;
  border-radius: 0.5rem;
  background-color: var(--color-main);
  transition: all 0.2s ease-in-out;
  margin: 0;
}

.app-button:hover {
  background-color: var(--color-main-dark);
}

.app-button:active {
  background-color: var(--color-main-light);
}
</style>
