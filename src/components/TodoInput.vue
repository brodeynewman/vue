<template>
  <form @submit="handleSubmit">
    <input v-model="value" placeholder="Do something..." />
  </form>
</template>

<script>
export default {
  name: 'TodoInput',
  data() {
    return {
      value: '',
    }
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault();

      if (!this.value) {
        return;
      }

      this.$emit('onSubmit', this.value);
      this.value = '';
    }
  }
}
</script>