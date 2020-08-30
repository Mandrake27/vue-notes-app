<template>
  <div class="new-note">
    <label style="height: 80px">
      Title
      <input v-model="note.title" type="text">
    </label>
    <priorities @setPriority="setPriority" />
    <label>
      Description
      <textarea v-model="note.description"></textarea>
    </label>
    <button @click="addNote" class="btn btnPrimary">New Note</button>
  </div>
</template>

<script>
import priorities from '@/components/Priorities';
export default {
  components: {
    priorities
  },
  props: {
    note: {
      type: Object,
      required: true,
    },
  },
  mounted() {
    this.setStandardChecked();
  },
  methods: {
    addNote() {
      const { priority } = this.note;
      if (priority !== 'standard') {
        const elem = document.getElementById(priority);
        this.setStandardChecked();
        elem.checked = false;
      }
      this.$emit('addNote', this.note);
    },
    setPriority(value) {
      this.note.priority = value;
    },
    setStandardChecked() {
      const standard = document.getElementById('standard');
      standard.checked = true;
    }
  }
}
</script>

<style lang="scss">
.priority-box {
  display: flex;
  justify-content: space-between;
  max-width: 300px;
  height: 10px;
  margin-bottom: 30px;
  margin-left: 10px;

  .radio-btn {
    position: relative;
    display: block;

    label {
      display: block;
      color: black;
      border-radius: 5px;
      padding: 10px 20px;
      margin-bottom: 5px;
      cursor: pointer;
      transition: all ease-in-out 0.2s;
    }

    input[type="radio"] {
      display: none;
      position: absolute;
      width: 100%;
      appearance: none;
      border: 1px solid white;

      &:checked + label {
        border: 1px solid black;
        transform: scale(1.1);
      }
    }
  }
}

.new-note {
  text-align: center;
}
</style>