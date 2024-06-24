<template>
  <div class="todo-list">
    <h1>Список задач</h1>
    <div class="add-todo">
      <input v-model="newTodoText" @keyup.enter="addTodo" placeholder="Добавить новую задачу" class="input">
      <button @click="addTodo" class="btn add-btn">Добавить</button>
    </div>
    <div class="filters">
      <label>
        Фильтер:
        <select v-model="filter" class="select">
          <option value="all">Все</option>
          <option value="completed">Завершенные</option>
          <option value="incomplete">Незавершенные</option>
        </select>
      </label>
      <label>
        Сортировка:
        <select v-model="sortOrder" class="select">
          <option value="none">По умолчанию</option>
          <option value="asc">По возрастанию</option>
          <option value="desc">По убыванию</option>
        </select>
      </label>
    </div>
    <div class="todos">
      <TodoItem 
        v-for="(todo, index) in filteredAndSortedTodos" 
        :key="index" 
        :todo="todo" 
        :index="index"
        @toggle-complete="toggleComplete"
        @edit-todo="editTodo"
        @delete-todo="deleteTodo"
      />
    </div>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue';

export default {
  components: { TodoItem },
  data() {
    return {
      newTodoText: '',
      todos: [
        {text: 'Learn Vue', completed: true},
        {text: 'Learn Vuex', completed: false},
        {text: 'Learn Vue Router', completed: false},
      ],
      filter: 'all',
      sortOrder: 'none'
    };
  },
  computed: {
    filteredAndSortedTodos() {
      let todos = this.todos;

      if (this.filter === 'completed') {
        todos = todos.filter(todo => todo.completed);
      } else if (this.filter === 'incomplete') {
        todos = todos.filter(todo => !todo.completed);
      }

      if (this.sortOrder === 'asc') {
        todos = todos.slice().sort((a, b) => a.text.localeCompare(b.text));
      } else if (this.sortOrder === 'desc') {
        todos = todos.slice().sort((a, b) => b.text.localeCompare(a.text));
      }

      return todos;
    }
  },
  methods: {
    addTodo() {
      if (this.newTodoText.trim() === '') return;
      this.todos.push({ text: this.newTodoText, completed: false });
      this.newTodoText = '';
    },
    toggleComplete(index) {
      this.todos[index].completed = !this.todos[index].completed;
    },
    editTodo(index, newText) {
      this.todos[index].text = newText;
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
    }
  }
}
</script>

<style scoped>
.todo-list {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h1 {
  font-size: 2em;
  margin-bottom: 20px;
}

.add-todo {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.input {
  flex-grow: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  transition: border-color 0.3s;
}

.input:focus {
  border-color: #4CAF50;
}

.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.add-btn {
  background-color: #4CAF50;
  color: white;
}

.add-btn:hover {
  background-color: #45a049;
}

.filters {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.select {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  transition: border-color 0.3s;
}

.select:focus {
  border-color: #4CAF50;
}

.todos {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

@media (max-width: 600px) {
  .todo-list {
    padding: 10px;
  }

  .add-todo, .filters {
    flex-direction: column;
    gap: 10px;
  }

  .input {
    width: 100%;
  }
}
</style>
