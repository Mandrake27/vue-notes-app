<template>
  <div class="notes">
    <div class="note" :class="{ wide: !grid }" v-for="(
        { title, description, date, priority, titleEdit, descEdit }, index) in notes" :key="index">
      <div class="note-header" :class="getItemClass(priority)">
        <div v-show="titleEdit === false">
          <p @dblclick="notes[index].titleEdit = true">{{ title }}</p>
        </div>
        <label>
          <input class="edit-title"
                 v-show="titleEdit === true"
                 v-model="title"
                 @keyup.enter="updateItem(index, 'title', title, 'titleEdit')"
                 @keyup.esc="notes[index].titleEdit = false"
                 autofocus
          >
        </label>
        <p class="close-button" @click="deletePost(index)">x</p>
      </div>
      <div class="note-body">
        <div v-show="descEdit === false">
          <p @dblclick="notes[index].descEdit = true">{{ description }}</p>
        </div>
        <label>
          <input class="edit-desc"
                 v-show="notes[index].descEdit === true"
                 v-model="description"
                 @keyup.enter="updateItem(index, 'description', description, 'descEdit')"
                 @keyup.esc="notes[index].descEdit = false; $emit('cancel')"
          >
        </label>
        <span>{{ date }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    notes: {
      type: Array,
      required: true,
    },
    grid: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    deletePost(index) {
      this.$emit("delete", index);
    },
    getItemClass(priority) {
      const isWide = this.grid ? '' : 'wide';
      switch (priority) {
        default:
        case 'standard':
          return `standard ${isWide}`;
        case 'medium':
          return `medium ${isWide}`;
        case 'high':
          return `high ${isWide}`;
      }
    },
    updateItem(index, key, value, editKey) {
      const newDate = new Date(Date.now()).toLocaleString();
      this.$emit('update', { index, key, value, newDate });
      this.notes[index][editKey] = false;
    },
  },
};
</script>

<style lang="scss">
.notes {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  padding: 40px 0;
}

.note {
  width: 48%;
  padding: 18px 20px;
  margin-bottom: 20px;
  background-color: #ffffff;
  border-radius: 15px;
  transition: all 0.25s cubic-bezier(0.02, 0.01, 0.47, 1);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.4);
  cursor: pointer;

  &:hover {
    box-shadow: 5px 20px 30px rgba(0, 0, 0, 0.4);
    transform: translate(0, -6px);
    transition-delay: 0s !important;
  }

  &.wide {
    width: 100%;
    text-align: center;
  }
}

.note-header {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-between;

  .close-button {
    position: absolute;
    top: 0;
    right: 0;
    margin-right: 0;
    cursor: pointer;
    transition: all 0.25s cubic-bezier(0.02, 0.01, 0.47, 1);

    &:hover {
      transform: translate(0, -6px);
      transition-delay: 0s !important;
    }
  }

  &.wide {
    width: 100%;
    justify-content: center;
  }

  &.medium {
    background-color: rgba(200, 51, 48, .4);

    p {
      color: white;
    }
  }

  &.high {
    background-color: rgba(0, 0, 0, .4);

    p {
      color: black;
    }
  }

  h1 {
    font-size: 32px;
  }

  p {
    font-size: 22px;
    color: #444ce0;
  }

  .edit-title {
    width: 250px;
    font-size: 22px;
    margin-bottom: 20px;
  }
}

.note-body {
  p {
    margin: 20px 0;
  }

  span {
    font-size: 14px;
    color: #999999;
  }

  .edit-desc {
    font-size: 14px;
    margin-top: 20px;
  }
}
</style>