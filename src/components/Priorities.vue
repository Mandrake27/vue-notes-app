<template>
  <div class="priority-box">
    <div class="radio-btn">
      <input type="radio"
             id="standard"
             name="priority"
             value="standard"
             @click="setPriority('standard')"
      />
      <label for="standard">Standard</label>
    </div>

    <div class="radio-btn">
      <input type="radio"
             id="medium"
             name="priority"
             value="medium"
             @click="setPriority('medium')"
      />
      <label for="medium">Medium</label>
    </div>

    <div class="radio-btn">
      <input type="radio"
             id="high"
             name="priority"
             value="high"
             @click="setPriority('high')"
      />
      <label for="high">High</label>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    setPriority(value) {
      this.$emit('setPriority', value);
    },
  }
}
</script>