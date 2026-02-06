<template>
  <div class="app-input-container">
    <label v-if="label" :for="id" class="app-label">
      {{ label }}
      <span v-if="required" class="required-marker">*</span>
    </label>
    
    <div class="input-wrapper" :class="{ 'has-icon': type === 'password' }">
      <input
        :id="id"
        :type="inputType"
        :value="modelValue"
        :placeholder="placeholder"
        :required="required"
        class="app-input"
        @input="$emit('update:modelValue', $event.target.value)"
      />
      
      <button
        v-if="type === 'password'"
        type="button"
        class="icon-button"
        @click="togglePassword"
        aria-label="Toggle password visibility"
      >
        <svg v-if="showPassword" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path><circle cx="12" cy="12" r="3"></circle></svg>
        <svg v-else xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.06M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path><line x1="1" y1="1" x2="23" y2="23"></line></svg>
      </button>
    </div>
    
    <span v-if="error" class="error-message">{{ error }}</span>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  modelValue: [String, Number],
  label: String,
  type: {
    type: String,
    default: 'text'
  },
  placeholder: String,
  required: Boolean,
  error: String,
  id: {
    type: String,
    default: () => `input-${Math.random().toString(36).substr(2, 9)}`
  }
});

defineEmits(['update:modelValue']);

const showPassword = ref(false);

const inputType = computed(() => {
  if (props.type === 'password') {
    return showPassword.value ? 'text' : 'password';
  }
  return props.type;
});

const togglePassword = () => {
  showPassword.value = !showPassword.value;
};
</script>

<style scoped>
.app-input-container {
  display: flex;
  flex-direction: column;
  gap: 8px;
  width: 100%;
}

.app-label {
  font-size: 14px;
  font-weight: 500;
  color: var(--text-main);
  display: flex;
  align-items: center;
  gap: 2px;
}

.required-marker {
  color: var(--error);
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.app-input {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  font-size: 15px;
  transition: all 0.2s ease;
  background-color: white;
}

.app-input:focus {
  outline: none;
  border-color: var(--border-focus);
  box-shadow: 0 0 0 2px var(--primary-light);
}

.app-input::placeholder {
  color: var(--text-muted);
}

.icon-button {
  position: absolute;
  right: 12px;
  color: var(--text-muted);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4px;
  border-radius: 4px;
}

.icon-button:hover {
  background-color: #f3f4f6;
  color: var(--text-main);
}

.error-message {
  font-size: 12px;
  color: var(--error);
  margin-top: 2px;
}
</style>
