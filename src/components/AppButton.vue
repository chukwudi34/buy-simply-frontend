<template>
  <button
    :class="['app-button', `variant-${variant}`]"
    :disabled="disabled || loading"
    :type="type"
    @click="$emit('click', $event)"
  >
    <span v-if="loading" class="loader"></span>
    <slot v-else></slot>
  </button>
</template>

<script setup>
defineProps({
  variant: {
    type: String,
    default: 'primary',
    validator: (value) => ['primary', 'ghost', 'link'].includes(value)
  },
  disabled: {
    type: Boolean,
    default: false
  },
  loading: {
    type: Boolean,
    default: false
  },
  type: {
    type: String,
    default: 'button'
  }
});

defineEmits(['click']);
</script>

<style scoped>
.app-button {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 12px 24px;
  border-radius: var(--radius-md);
  font-size: 16px;
  font-weight: 600;
  transition: all 0.2s ease;
  width: 100%;
  gap: 8px;
}

.app-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.variant-primary {
  background-color: var(--primary);
  color: var(--text-white);
}

.variant-primary:hover:not(:disabled) {
  background-color: var(--primary-hover);
}

.variant-ghost {
  background-color: transparent;
  color: var(--primary);
  border: 1px solid var(--primary);
}

.variant-ghost:hover:not(:disabled) {
  background-color: var(--primary-light);
}

.variant-link {
  background-color: transparent;
  color: var(--primary);
  padding: 0;
  width: auto;
  font-size: 14px;
}

.variant-link:hover:not(:disabled) {
  text-decoration: underline;
}

.loader {
  width: 20px;
  height: 20px;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  border-top-color: white;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
</style>
