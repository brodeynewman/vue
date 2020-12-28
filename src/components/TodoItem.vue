<template>
  <li :class="{ item: true, completed: isCompleted }">
    <span class="name">{{ name }}</span>
    <div>
      <button class="button" @click="handleDelete">
        <svg height="20" width="20" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
      </button>
      <button class="button" @click="handleComplete">
        <svg height="20" width="20" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
      </button>
    </div>
  </li>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    id: Number,
    name: String,
    isCompleted: Boolean, 
  },
  methods: {
    handleDelete() {
      this.$emit('onDelete', this.id);
    },
    handleComplete() {
      this.$emit('onComplete', this.id);
    }
  }
}
</script>

<style>
  .name {
    display: block;
  }

  .item {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .button {
    color: #696969;
    background-color: transparent;
    border: none;
    cursor: pointer;
    padding: 0;
    margin-left: 15px;
  }

  .completed {
    opacity: .5;
    text-decoration: line-through;
  }
</style>