<template>
  <div>
    <h1>Todo List</h1>

    <TodoInput @onSubmit="handleSubmit" />
  </div>
  <div v-if="todos.length > 0" class="todo-container">
    <div class="items">
      <h3>
        <span>Items</span>
        <span v-if="filter === 'completed'" class="amount">
          {{ amountCompleted }}
        </span>
        <span v-if="filter === 'all'" class="amount">
          {{ amountAll }}
        </span>
      </h3>

      <div>
        <button
          @click="handleFilterChange('all')"
          :class="{ 'active-filter': filter === 'all', filter: true }"
        >
          All
        </button>
        <button
          @click="handleFilterChange('completed')"
          :class="{ 'active-filter': filter === 'completed', filter: true }"
        >
          Completed
        </button>
      </div>
    </div>

    <ul v-for="todo in todoList" :key="todo.id" class="list">
      <TodoItem
        @onDelete="handleDelete"
        @onComplete="handleComplete"
        :name="todo.title"
        :id="todo.id"
        :isCompleted="todo.isCompleted"
      />
    </ul>
  </div>
</template>

<script>
import fp from 'lodash/fp';
import TodoInput from './TodoInput';
import TodoItem from './TodoItem';

export default {
  name: 'TodoContainer',
  data() {
    return {
      todos: [],
      filter: 'all',
    };
  },
  components: {
    TodoInput,
    TodoItem,
  },
  computed: {
    todoList() {
      // we assume there will only ever be 2 filters
      return this.filter === 'all' ? this.todos : this.todos.filter(fp.get('isCompleted'));
    },
    amountCompleted() {
      const list = this.todos.filter(fp.get('isCompleted'));

      return list.length;
    },
    amountAll() {
      return this.todos.length;
    }
  },
  methods: {
    handleDelete(deleted) {
      this.todos = this.todos.filter(({ id }) => id !== deleted);
    },
    handleComplete(completed) {
      this.todos = this.todos.map(item => item.id === completed
        ? { ...item, isCompleted: !item.isCompleted }
        : item
      );
    },
    handleFilterChange(type) {
      this.filter = type;
    },
    handleSubmit(data) {
      this.todos = [
        ...this.todos,
        {
          title: data,
          id: this.todos.length + 1,
          isCompleted: false,
        }
      ]
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .list {
    padding: 0;
    list-style: none;
  }

  .items {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .filter {
    opacity: .5;
    border: 1px solid #ddd;
    background-color: transparent;
    color: #696969;
    font-size: 14px;
    margin-left: 15px;
    border-radius: 3px;
    cursor: pointer;
  }

  .active-filter {
    opacity: 1;
  }

  .todo-container {
    text-align: left;
    max-width: 350px;
    margin: auto;
    margin-top: 25px; 
  }

  .amount {
    font-size: 14px;
    color: #888;
    padding-left: 5px;
  }
</style>
