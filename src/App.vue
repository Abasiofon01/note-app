<template>
  <div class="wrapper">
    <!-- Form for Creating Notes -->
    <div class="create-note-form">
      <NoteForm @submit="addNote" ref="createNoteForm" />
    </div>

    <!-- Notes Grid -->
    <div class="notes-grid">
      <NoteCard
        v-for="note in notes"
        :key="note.id"
        :note="note"
        @edit-note="openEditor"
      />
    </div>

    <!-- Modal for Editing Notes -->
    <NoteEditor
      v-if="isEditorOpen"
      :show="isEditorOpen"
      @close="closeEditor"
      :noteColor="currentNote.color"
    >
      <NoteForm
        :note="currentNote"
        editMode
        @submit="updateNote"
        @delete="deleteNote"
        @close="closeEditor"
        ref="editNoteForm"
      />
    </NoteEditor>
  </div>
  <Toast ref="toast" />
</template>

<script>
import NoteForm from "./components/NoteForm.vue";
import NoteCard from "./components/NoteCard.vue";
import NoteEditor from "./components/NoteEditor.vue";
import { supabase } from "./supabase";
import Toast from "./components/Toast.vue";

export default {
  components: { NoteForm, NoteCard, NoteEditor, Toast },
  data() {
    return {
      notes: [],
      currentNote: null,
      isEditorOpen: false,
    };
  },
  methods: {
    // Add a new note
    async addNote(newNote) {
      try {
        const { data, error } = await supabase
          .from("notes")
          .insert([{ ...newNote }])
          .select("*");

        if (error) {
          this.$refs.toast.showToast("Failed to add note.", "error");
          throw error;
        }

        if (!data || data.length === 0) {
          this.$refs.toast.showToast(
            "No data returned from Supabase.",
            "error"
          );
          throw new Error("No data returned from Supabase.");
        }

        // Add the new note at the top of the notes array
        this.notes.unshift({ ...newNote, id: data[0].id });
        this.$refs.toast.showToast("Note added successfully!", "success");

        // Reset the form after successful note creation
        this.$refs.createNoteForm.resetForm();
      } catch (error) {
        console.error("Error adding note:", error.message);
      }
    },

    // Open the editor for an existing note
    openEditor(note) {
      this.currentNote = { ...note };
      this.isEditorOpen = true;
    },

    // Update an existing note
    async updateNote(updatedNote) {
      try {
        const { error } = await supabase
          .from("notes")
          .update({
            title: updatedNote.title,
            content: updatedNote.content,
            color: updatedNote.color,
          })
          .eq("id", updatedNote.id);

        if (error) {
          this.$refs.toast.showToast("Failed to update note.", "error");
          throw error;
        }

        // Update the note in the notes array and push it to the front
        const index = this.notes.findIndex((n) => n.id === updatedNote.id);
        if (index !== -1) {
          this.notes[index] = updatedNote;
          this.notes.unshift(this.notes.splice(index, 1)[0]); // Move updated note to the front
        }

        this.closeEditor();
        this.$refs.toast.showToast("Note updated successfully!", "success");

        // Reset the form after successful update
        this.$refs.editNoteForm.resetForm();
      } catch (error) {
        console.error("Error updating note:", error.message);
      }
    },

    // Delete a note
    async deleteNote(noteToDelete) {
      try {
        const { error } = await supabase
          .from("notes")
          .delete()
          .eq("id", noteToDelete.id);

        if (error) {
          this.$refs.toast.showToast("Failed to delete note.", "error");
          throw error;
        }

        // Remove the note from the list
        this.notes = this.notes.filter((note) => note.id !== noteToDelete.id);
        this.closeEditor();
        this.$refs.toast.showToast("Note deleted successfully!", "success");

        // Reset the form after deletion
        this.$refs.noteForm.resetForm();
      } catch (error) {
        console.error("Error deleting note:", error.message);
      }
    },

    async fetchNotes() {
      try {
        const { data, error } = await supabase
          .from("notes")
          .select("*")
          .order("updated_at", { ascending: false });
        if (error) throw error;

        this.notes = data; // Initialize notes with data from Supabase
      } catch (error) {
        console.error("Error fetching notes:", error.message);
      }
    },

    closeEditor() {
      this.isEditorOpen = false;
      this.currentNote = null;
    },
  },

  async mounted() {
    await this.fetchNotes();
  },
};
</script>
