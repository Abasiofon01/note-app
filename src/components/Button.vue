<template>
  <button
    :class="[
      'custom-button',
      color,
      size,
      { 'is-loading': loading, 'is-outline': outline, 'is-flat': flat },
    ]"
    :disabled="loading || disabled"
    :type="type"
    @click="onClick"
  >
    <span v-if="loading" class="spinner"></span>
    <span v-if="icon && !loading" :class="['icon', icon]"></span>
    <span v-if="!loading">{{ label }}</span>
  </button>
</template>

<script>
export default {
  name: "CustomButton",
  props: {
    label: {
      type: String,
      default: "Click Me",
    },
    color: {
      type: String,
      default: "primary",
    },
    size: {
      type: String,
      default: "medium",
    },
    type: {
      type: String,
      default: "button",
    },
    icon: {
      type: String,
      default: "",
    },
    loading: {
      type: Boolean,
      default: false,
    },
    outline: {
      type: Boolean,
      default: false,
    },
    flat: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    onClick() {
      this.$emit("click");
    },
  },
};
</script>

<style scoped>
.custom-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 10px 16px;
  font-size: 16px;
  font-weight: bold;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

.custom-button:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.custom-button.primary {
  background-color: #1976d2;
  color: white;
}

.custom-button.secondary {
  background-color: #424242;
  color: white;
}

.custom-button.success {
  background-color: #388e3c;
  color: white;
}

.custom-button.danger {
  background-color: #d32f2f;
  color: white;
}

.custom-button.medium {
  font-size: 16px;
  padding: 10px 20px;
}

.custom-button.small {
  font-size: 14px;
  padding: 8px 16px;
}

.custom-button.large {
  font-size: 18px;
  padding: 12px 24px;
}

.custom-button.is-loading {
  cursor: not-allowed;
}

.custom-button.is-outline {
  background-color: transparent;
  border: 2px solid currentColor;
}

.custom-button.is-flat {
  background-color: transparent;
  color: currentColor;
}

.custom-button.is-flat:hover {
  background-color: rgba(0, 0, 0, 0.1);
}

.custom-button.primary.is-flat:hover {
  background-color: rgba(25, 118, 210, 0.1);
}

.custom-button.secondary.is-flat:hover {
  background-color: rgba(66, 66, 66, 0.1);
}

.custom-button.success.is-flat:hover {
  background-color: rgba(56, 142, 60, 0.1);
}

.custom-button.danger.is-flat:hover {
  background-color: rgba(211, 47, 47, 0.1);
}

.custom-button .spinner {
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-top: 2px solid white;
  border-radius: 50%;
  width: 16px;
  height: 16px;
  animation: spin 1s linear infinite;
}

.custom-button .icon {
  margin-right: 8px;
}

.custom-button .icon::before {
  content: attr(class);
  font-family: "Material Icons";
  font-size: 20px;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
