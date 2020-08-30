<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <div id="app">
            <!-- message -->
            <message v-if="message" :message="message"/>
            <!-- new note -->
            <newNote :note="note" @addNote="addNote"/>
            <!-- notes -->
            <div class="note-header">
              <!-- title -->
              <div class="header-left-side">
                <h1>{{ title }}</h1>
                <!--search -->
                <search :value="search" placeholder="Search Cards" @search="search = $event"/>
              </div>
              <!-- icon controls -->
              <div class="icons">
                <svg
                    :class="{ active: grid }"
                    @click="grid = true"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                >
                  <rect x="3" y="3" width="7" height="7"/>
                  <rect x="14" y="3" width="7" height="7"/>
                  <rect x="14" y="14" width="7" height="7"/>
                  <rect x="3" y="14" width="7" height="7"/>
                </svg>
                <svg
                    :class="{ active: !grid }"
                    @click="grid = false"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                >
                  <line x1="8" y1="6" x2="21" y2="6"/>
                  <line x1="8" y1="12" x2="21" y2="12"/>
                  <line x1="8" y1="18" x2="21" y2="18"/>
                  <line x1="3" y1="6" x2="3" y2="6"/>
                  <line x1="3" y1="12" x2="3" y2="12"/>
                  <line x1="3" y1="18" x2="3" y2="18"/>
                </svg>
              </div>
            </div>
            <notes :notes="getFilteredNotes" :grid="grid" @update="updateItem" @delete="deleteNotes"/>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import message from "@/components/Message";
import newNote from "@/components/NewNote";
import notes from "@/components/Notes";
import search from "@/components/Search";

export default {
  components: {
    message,
    newNote,
    notes,
    search,
  },
  data() {
    return {
      title: "Notes App",
      message: null,
      grid: true,
      search: '',
      note: {
        title: "",
        description: "",
        priority: "standard"
      },
      notes: [
        {
          priority: 'standard',
          title: "First Note",
          description: "Description for first note",
          date: new Date(Date.now()).toLocaleString(),
          titleEdit: false,
          descEdit: false,
        },
        {
          priority: 'standard',
          title: "Second Note",
          description: "Description for second note",
          date: new Date(Date.now()).toLocaleString(),
          titleEdit: false,
          descEdit: false,
        },
        {
          priority: 'standard',
          title: "Third Note",
          description: "Description for third note",
          date: new Date(Date.now()).toLocaleString(),
          titleEdit: false,
          descEdit: false,
        },
      ],
    };
  },
  computed: {
    getFilteredNotes() {
       let array = this.notes,
           search = this.search;
       if (!search) return array;
       search = search.trim().toLowerCase();
       array = array.filter(item => {
         if (item.title.toLowerCase().indexOf(search) !== -1) {
           return item;
         }
       });
       return array;
    }
  },
  methods: {
    addNote() {
      const { title, description, priority } = this.note;
      if (title && description) {
        this.notes.push({
          priority,
          title,
          description,
          date: new Date(Date.now()).toLocaleString(),
          titleEdit: false,
          descEdit: false,
        });
        this.message = null;
        return (this.note = {
          title: "",
          description: "",
          priority: "standard",
        });
      } else {
        this.message = "Fill both areas!";
      }
    },
    deleteNotes(index) {
      this.notes.splice(index, 1);
    },
    updateItem({ key, value, index, newDate }) {
      this.notes[index][key] = value;
      this.notes[index].date = newDate;
    }
  },
};
</script>

<style lang="scss">
.note-header {
  margin-top: 20px;

  .header-left-side {
    display: flex;
    justify-content: space-between;
    width: 615px;
  }

  .icons {
    display: flex;
    justify-content: space-between;
    max-width: 60px;
    width: 100%;

    svg {
      color: #999999;
      cursor: pointer;
    }

    .active {
      color: black;
    }
  }
}
</style>
