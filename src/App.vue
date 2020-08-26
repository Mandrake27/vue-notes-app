<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">
          <div id="app">
            <h1>{{ title }}</h1>
            <div class="message" v-if="message">
              <p>{{ message }}</p>
            </div>
            <!--new note-->
            <div class="new-note">
              <input v-model="note.title" type="text">
              <textarea v-model="note.description"></textarea>
              <button @click="addNote">New Note</button>
            </div>
            <!--note list-->
            <div class="notes">
              <div class="note" v-for="(note, index) in notes" :key="index">
                <div class="note-header">
                  <p>{{ note.title }}</p>
                </div>
                <div class="note-body">
                  <p>{{ note.description }}</p>
                  <span>{{ note.date }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: 'Notes App',
      message: null,
      note: {
        title: '',
        description: ''
      },
      notes: [
        {
          title: 'First Note',
          description: 'Description for first note',
          date: new Date(Date.now()).toLocaleString()
        },
        {
          title: 'Second Note',
          description: 'Description for second note',
          date: new Date(Date.now()).toLocaleString()
        },
        {
          title: 'Third Note',
          description: 'Description for third note',
          date: new Date(Date.now()).toLocaleString()
        }
      ]
    }
  },
  methods: {
    addNote() {
      const { title, description } = this.note;
      if (title && description) {
        this.notes.push({
          title,
          description,
          date: new Date(Date.now()).toLocaleString()
        });
        this.message = null;
        return this.note = {};
      } else {
        this.message = 'Fill both areas!'
      }
    }
  }
};
</script>

<style>
</style>
