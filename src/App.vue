<template>
  <div id="app">
    <section class="container">
      <form @submit.prevent="addTodo">
        <div class="container__create-todo create-todo wrapper">
          <div class="create-todo__header">
            <h3 class="create-todo__title">TODO list: {{ todos.length }}</h3>
            <md-button
              type="submit"
              class="md-raised md-accent"
              :disabled="buttonCreateDisabled"
            >
              Add
            </md-button>
          </div>
          <md-field class="create-todo__input-title">
            <label>Заголовок - ({{ title.length }})</label>
            <md-input type="text" v-model="title" require />
            <span class="md-error">Invalid first name</span>
          </md-field>
          <md-field class="create-todo__input-description">
            <label>Описание - ({{ this.description.length }})</label>
            <md-textarea v-model="description" />
          </md-field>
        </div>
      </form>
      <div class="container__todo-list todo-list wrapper">
        <ul class="todo-list__item" v-if="todos.length">
          <todo-element
            v-for="(todo, i) in todos"
            :key="todo.id"
            :index="i"
            :todo="todo"
            @delete-todo="deleteTodo"
            @save-todo="saveTodo"
          />
        </ul>
        <span class="todo-list__empty" v-else>Пусто</span>
      </div>
    </section>
  </div>
</template>

<script>
import TodoElement from "@/components/TodoElement";

export default {
  name: "App",
  components: { TodoElement },
  data() {
    return {
      todos: [],
      title: "",
      description: "",
      invalid: true,
      buttonCreateDisabled: true
    };
  },
  watch: {
    description() {
      if (this.description.length >= 300) {
        this.buttonCreateDisabled = true;
      }
    },
    title() {
      if (this.title.length >= 3 && this.title.length <= 64) {
        this.buttonCreateDisabled = false;
      } else {
        this.buttonCreateDisabled = true;
      }
    },
  },
  created() {
    if (localStorage.getItem("todos")) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    }
  },
  methods: {
    addTodo() {
      const createId = Date.now();

      this.todos.push({
        id: createId,
        title: this.title,
        date: new Date().toLocaleString(navigator.language, {
          year: "numeric",
          month: "long",
          day: "numeric",
          hour: "numeric",
          minute: "numeric"
        }),
        description: this.description
      });
      this.title = "";
      this.description = "";
      this.saveLocalStorageTodos();
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(t => t.id !== id);
      this.saveLocalStorageTodos();
    },
    saveTodo() {
      this.saveLocalStorageTodos();
    },
    saveLocalStorageTodos() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
      this.totos = JSON.parse(localStorage.getItem("todos"));
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  background: rgba(29, 29, 29, 0.2);
  width: 100vw;
  height: 100vh;
  padding-top: 40px;
}

.create-todo {
  &__header {
    display: flex;
    justify-content: space-around;
    align-items: center;
  }

  &__title {
    font-size: 30px;
    color: #0040ea;
    margin: 0;
  }

  &__input-title {
    margin: 0;
    height: 50px;
  }
}
.todo-list {
  height: 410px;
  overflow: auto;
  background-color: #dddddd;
  box-shadow: 20px 15px 10px 5px rgba(0, 0, 0, 0.5);
  position: relative;
  &__item {
    padding: 10px;
  }
  &__empty {
    position: absolute;
    font-size: 30px;
    align-self: center;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}

.wrapper {
  width: 500px;
  margin: 0 auto;
}
</style>
