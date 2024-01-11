<template>
  <button
      class="btn border border-black rounded-lg"
      :class="addClass"
      v-if="title"
      @click="sendClick"
  >
    {{ title }}
  </button>
</template>

<script>
export default {
  props: ['title', 'addClass'],
  methods: {
    sendClick() {
      this.$emit('buttonClick');
    }
  }
}
</script>

<style scoped>
.btn {
  background-color: white;
  position: relative;
  padding: 5px 14px;
}
</style>
