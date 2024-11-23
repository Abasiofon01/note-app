<template>
  <div
    class="note-card"
    :style="{
      backgroundColor: note.color || '#FFFFFF',
      borderWidth: '1px',
      borderStyle: 'solid',
      borderColor: note.color === '#FFFFFF' ? '#d3d3d3' : note.color,
    }"
    @mouseenter="handleMouseEnter"
    @mouseleave="handleMouseLeave"
    @click="editNote"
  >
    <div class="note-content title">
      <h3 class="note-title">{{ note.title }}</h3>
    </div>
    <div class="note-content text">
      <p class="note-text">{{ note.content }}</p>
    </div>
    <button
      v-show="showEditIcon"
      class="edit-icon"
      @click.stop="editNote"
      aria-label="Edit Note"
    >
      <i class="las la-pen"></i>
    </button>
  </div>
</template>

<script>
export default {
  name: "NoteCard",
  props: {
    note: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      isHovered: false,
      showEditIcon: false,
      hoverTimeout: null,
    };
  },
  methods: {
    handleMouseEnter() {
      clearTimeout(this.hoverTimeout);
      this.isHovered = true;
      this.hoverTimeout = setTimeout(() => {
        this.showEditIcon = true;
      }, 200);
    },
    handleMouseLeave() {
      clearTimeout(this.hoverTimeout);
      this.isHovered = false;
      this.hoverTimeout = setTimeout(() => {
        this.showEditIcon = false;
      }, 200);
    },
    editNote() {
      this.$emit("edit-note", this.note);
    },
  },
};
</script>

<style scoped>
.note-card {
  position: relative;
  padding: 1rem;
  border-radius: 10px;
  /* box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); */
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.note-card:hover {
  transform: scale(1.02);
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.15);
}

.note-content {
  pointer-events: none;
}

.note-content.title {
  align-items: center;
  display: flex;
  flex-wrap: wrap;
  font-size: 1.25rem;
  font-weight: 500;
  letter-spacing: 0.0125em;
  line-height: 2rem;
  word-break: break-all;
}

.edit-icon {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: rgba(0, 0, 0, 0.4);
  color: #fff;
  border: none;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  cursor: pointer;
  opacity: 0;
  transform: scale(0.8);
  transition: opacity 0.1s ease, transform 0.1s ease;
  pointer-events: auto;
}

.note-card:hover .edit-icon {
  opacity: 1;
  transform: scale(1);
}

.edit-icon:hover {
  background-color: rgba(0, 0, 0, 0.3);
}
</style>
