<template>
  <div class="login-container">
    <!-- Left Section (Branding & Imagery) -->
    <div class="branding-section">
      <div class="content-wrapper">
        <header class="logo-header">
          <img src="/asset/images/logo.png" alt="Team Achieve Logo" class="logo" />
        </header>

        <div class="image-container">
          <img 
            src="/asset/images/dance_demostrate.png" 
            alt="Smiling couple outdoors" 
            class="main-image"
          />
        </div>

        <div class="branding-footer">
          <h2 class="branding-title">Team Achieve</h2>
          <p class="branding-subtitle">Your perfect solution for funding your desires</p>
        </div>
      </div>
    </div>

    <!-- Right Section (Form) -->
    <div class="form-section">
      <div class="form-wrapper">
        <div class="form-header">
          <h1 class="welcome-title">Welcome Back</h1>
          <p class="welcome-subtitle">
            Enter your email address and password to access your account.
          </p>
        </div>

        <form @submit.prevent="handleSignIn" class="login-form" novalidate>
          <AppInput
            v-model="email"
            label="Email Address"
            type="email"
            placeholder="Enter your email"
            required
            :error="errors.email"
          />

          <AppInput
            v-model="password"
            label="Password"
            type="password"
            placeholder="Enter your password"
            required
            :error="errors.password"
          />

          <div class="form-options">
            <label class="checkbox-container">
              <input type="checkbox" v-model="rememberMe" />
              <span class="checkmark"></span>
              <span class="checkbox-label">Remember me</span>
            </label>
            <a href="#" class="forgot-password">Forgot Password?</a>
          </div>

          <AppButton type="submit" :loading="isLoggingIn">
            Sign in
          </AppButton>
        </form>

        <div class="form-footer">
          <p>
            Don't have an account? 
            <a href="#" class="signup-link">Sign up</a>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import AppInput from '../components/AppInput.vue';
import AppButton from '../components/AppButton.vue';

const email = ref('');
const password = ref('');
const rememberMe = ref(false);
const isLoggingIn = ref(false);
const errors = ref({
  email: '',
  password: ''
});

const validateForm = () => {
  let isValid = true;
  errors.value = { email: '', password: '' };

  // Email validation
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!email.value) {
    errors.value.email = 'Email address is required';
    isValid = false;
  } else if (!emailRegex.test(email.value)) {
    errors.value.email = 'Please enter a valid email address';
    isValid = false;
  }

  // Password validation
  if (!password.value) {
    errors.value.password = 'Password is required';
    isValid = false;
  } else if (password.value.length < 6) {
    errors.value.password = 'Password must be at least 6 characters';
    isValid = false;
  }

  return isValid;
};

const handleSignIn = () => {
  if (!validateForm()) return;

  isLoggingIn.value = true;
  // Simulate API call
  setTimeout(() => {
    isLoggingIn.value = false;
    alert('Logged in successfully!');
  }, 1500);
};
</script>

<style scoped>
.login-container {
  display: flex;
  min-height: 100vh;
  width: 100%;
}

/* Branding Section Styles */
.branding-section {
  flex: 1;
  background-color: var(--bg-sidebar);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 40px;
}

.content-wrapper {
  max-width: 500px;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 32px;
}

.logo-header {
  margin-bottom: 20px;
}

.logo {
  height: 180px;
  width: auto;
  object-fit: contain;
}

.image-container {
  width: 100%;
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-md);
}

.main-image {
  width: 100%;
  height: auto;
  display: block;
}

.branding-footer {
  text-align: center;
}

.branding-title {
  color: var(--primary);
  font-size: 24px;
  font-weight: 700;
  margin-bottom: 8px;
}

.branding-subtitle {
  color: var(--text-muted);
  font-size: 16px;
  line-height: 1.5;
}

/* Form Section Styles */
.form-section {
  flex: 1;
  background-color: var(--bg-main);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 40px;
}

.form-wrapper {
  max-width: 440px;
  width: 100%;
}

.form-header {
  text-align: center;
  margin-bottom: 32px;
}

.welcome-title {
  color: var(--primary);
  font-size: 32px;
  font-weight: 700;
  margin-bottom: 12px;
}

.welcome-subtitle {
  color: var(--text-muted);
  font-size: 15px;
  line-height: 1.6;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.form-options {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 14px;
}

.checkbox-container {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  user-select: none;
}

.checkbox-label {
  color: var(--text-main);
}

.forgot-password {
  font-weight: 500;
}

.form-footer {
  margin-top: 32px;
  text-align: center;
  font-size: 14px;
  color: var(--text-muted);
}

.signup-link {
  font-weight: 600;
}

/* Custom Checkbox Styling */
.checkbox-container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkmark {
  height: 18px;
  width: 18px;
  background-color: #fff;
  border: 1px solid var(--border-color);
  border-radius: 4px;
  position: relative;
  transition: all 0.2s;
}

.checkbox-container:hover input ~ .checkmark {
  border-color: var(--primary);
}

.checkbox-container input:checked ~ .checkmark {
  background-color: var(--primary);
  border-color: var(--primary);
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
  left: 6px;
  top: 2px;
  width: 4px;
  height: 8px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.checkbox-container input:checked ~ .checkmark:after {
  display: block;
}

/* Responsive Styles */
@media (max-width: 1024px) {
  .branding-section {
    padding: 30px;
  }
}

@media (max-width: 868px) {
  .login-container {
    flex-direction: column;
  }

  .branding-section {
    display: none; /* In many mobile views, branding is simplified or logo is moved to form section */
  }
  
  /* Mobile approach: Add logo to form section */
  .form-header::before {
    content: "";
    display: block;
    height: 120px;
    width: 100%;
    margin: 0 auto 32px;
    background-image: url('/asset/images/logo.png');
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
  }

  .form-section {
    padding: 60px 24px;
    min-height: 100vh;
  }

  .welcome-title {
    font-size: 28px;
  }
}
</style>
