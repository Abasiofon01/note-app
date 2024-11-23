<template>
  <div class="toast-container">
    <transition-group name="toast" tag="div">
      <div
        v-for="(toast, index) in toasts"
        :key="index"
        :class="`toast ${toast.type}`"
      >
        {{ toast.message }}
      </div>
    </transition-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      toasts: [],
    };
  },
  methods: {
    showToast(message, type = "success", duration = 3000) {
      const id = Date.now();
      this.toasts.push({ id, message, type });
      setTimeout(() => {
        this.toasts = this.toasts.filter((toast) => toast.id !== id);
      }, duration);
    },
  },
};
</script>

<style scoped>
.toast-container {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 1000;
}

.toast {
  padding: 10px 20px;
  margin-bottom: 10px;
  min-height: 50px;
  border-radius: 5px;
  color: #fff;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  font-size: 14px;
  font-weight: bold;
}

.toast.success {
  background-color: #4caf50;
}

.toast.error {
  background-color: #f44336;
}

.toast.warning {
  background-color: #ff9800;
}

.toast.info {
  background-color: #2196f3;
}

.toast-enter-active,
.toast-leave-active {
  transition: opacity 0.3s;
}

.toast-enter,
.toast-leave-to {
  opacity: 0;
}
</style>
