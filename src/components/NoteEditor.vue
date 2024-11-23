<template>
  <div v-if="show" class="modal-overlay">
    <div
      class="modal-content"
      :style="{ backgroundColor: noteColor || '#FFFFFF' }"
    >
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "NoteEditor",
  props: {
    show: {
      type: Boolean,
      required: true,
    },
    noteColor: {
      type: String,
      default: "#FFFFFF",
    },
  },
};
</script>
