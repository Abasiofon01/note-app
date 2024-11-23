<template>
  <form
    @submit.prevent="handleSubmit"
    class="note-form"
    :style="{ backgroundColor: formData.color || '#FFFFFF' }"
  >
    <div class="form-group title" v-if="editMode || showTitle">
      <input
        v-model="formData.title"
        maxlength="30"
        placeholder="Title"
        class="input"
      />
    </div>
    <div class="form-group content" @click="showTitle = true">
      <input
        v-model="formData.content"
        maxlength="100"
        placeholder="Write your note..."
        class="input"
        required
      />
    </div>
    <div class="form-actions" v-show="editMode || showTitle">
      <div class="left-items">
        <div class="form-group color-picker-wrapper">
          <button
            type="button"
            class="color-toggle-btn"
            @mouseenter="showColors = true"
            @mouseleave="hideColorsWithDelay"
          >
            <i class="las la-palette"></i>
          </button>
          <div
            class="color-picker"
            v-show="showColors"
            @mouseenter="clearHideDelay"
            @mouseleave="hideColorsWithDelay"
          >
            <button
              :title="colorObj.name"
              v-for="colorObj in colors"
              :key="colorObj.color"
              :style="{ backgroundColor: colorObj.color }"
              @click.prevent="setColor(colorObj.color)"
              class="color-option"
              :class="{ selected: formData.color === colorObj.color }"
            ></button>
          </div>
        </div>

        <button
          v-if="editMode"
          type="button"
          class="btn delete"
          @click="handleDelete"
          :disabled="loading"
        >
          <i class="las la-trash-alt"></i>
          <span v-if="loading">Deleting...</span>
        </button>
      </div>
      <div class="right-items">
        <Button
          size="small"
          color="secondary"
          :label="`${editMode ? 'Save' : 'Add Note'}`"
          type="submit"
          :flat="true"
          :disabled="loading"
        >
          <span v-if="loading">Saving...</span>
        </Button>
        <Button
          size="small"
          color="secondary"
          v-if="editMode"
          label="Close"
          type="button"
          :flat="true"
          @click="close"
          :disabled="loading"
        />
      </div>
    </div>
  </form>
</template>

<script>
import Button from "./Button.vue";

export default {
  name: "NoteForm",
  props: {
    note: {
      type: Object,
      default: () => ({ title: "", content: "", color: "#FFFFFF" }),
    },
    editMode: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    Button,
  },
  emits: ["submit", "delete", "close"],
  data() {
    return {
      formData: { ...this.note },
      colors: [
        { color: "#FFFFFF", name: "White" },
        { color: "#FF8A80", name: "Red" },
        { color: "#FFAB40", name: "Orange" },
        { color: "#FFFF8D", name: "Yellow" },
        { color: "#CCFF90", name: "Green" },
        { color: "#B2EBF2", name: "Blue" },
        { color: "#E1BEE7", name: "Purple" },
        { color: "#F8BBD0", name: "Pink" },
      ],
      showTitle: false,
      showColors: false,
      hideDelay: null,
      loading: false,
    };
  },
  methods: {
    async handleSubmit() {
      this.loading = true;
      try {
        await this.$emit("submit", this.formData);
      } finally {
        this.loading = false;
      }
    },
    async handleDelete() {
      this.loading = true;
      try {
        await this.$emit("delete", this.formData);
      } finally {
        this.loading = false;
      }
    },
    close() {
      this.$emit("close");
    },
    setColor(color) {
      this.formData.color = color;
      this.showColors = false;
    },
    hideColorsWithDelay() {
      this.hideDelay = setTimeout(() => {
        this.showColors = false;
      }, 200);
    },
    clearHideDelay() {
      clearTimeout(this.hideDelay);
    },
    resetForm() {
      this.formData = {
        title: "",
        content: "",
        color: "#FFFFFF",
      };
      this.showTitle = false;
      this.showColors = false;
    },
  },
};
</script>
