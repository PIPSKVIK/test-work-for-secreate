<template>
  <li class="todo-item">
    <span class="todo-item__index"># {{ index + 1 }}</span>
    <div class="todo-item__content content">
      <md-field v-if="!isTextEditDisabled" class="content__title">
        <md-input v-model="localTodo.title" :disabled="isTextEditDisabled" />
      </md-field>
      <span class="content__title-view" v-else>{{ localTodo.title }}</span>
      <div class="todo-item__control control">
        <div class="control__all-button">
          <md-button
            @click="$emit('delete-todo', localTodo.id)"
            class="md-icon-button control__del"
          >
            <md-icon :md-src="require('./icon/icon-delete.svg')" />
          </md-button>
          <md-button
            @click="handleSave"
            class="md-icon-button control__save"
            :disabled="isDisabled"
            :class="{ 'button-disabled': isDisabled }"
          >
            <md-icon :md-src="require('./icon/icon-save.svg')" />
          </md-button>
          <md-button
            class="md-icon-button control__edit"
            @click="isTextEditDisabled = !isTextEditDisabled"
          >
            <md-icon :md-src="require('./icon/icon-edit.svg')" />
          </md-button>
        </div>
        <span class="control__date">{{ localTodo.date }}</span>
      </div>
    </div>
    <md-field class="todo-item description" v-if="!isTextEditDisabled">
      <md-textarea
        class="description__element"
        v-model="localTodo.description"
        :disabled="isTextEditDisabled"
      />
    </md-field>
    <div class="description__element-preview" v-else>{{ localTodo.description }}</div>
  </li>
</template>

<script>
export default {
  name: "TodoElement",
  components: {},
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: Number
  },
  data() {
    return {
      localTodo: this.todo,
      isTextEditDisabled: true,
      isDisabled: true
    };
  },
  watch: {
    localTodo: {
      deep: true,
      handler() {
        this.isDisabled = false;
      }
    }
  },
  methods: {
    handleSave() {
      this.isDisabled = true;
      this.isTextEditDisabled = true;
      this.$emit("save-todo");
    }
  }
};
</script>

<style lang="scss" scoped>
.todo-item {
  list-style: none;
  border-bottom: 1px solid black;
  padding: 5px;
  &__index {
    font-size: 20px;
  }
}

.content {
  display: flex;
  justify-content: space-between;
  &__title-view {
    color: cornflowerblue;
    font-size: 18px;
    text-transform: uppercase;
  }
}

.control {
  display: flex;
  flex-direction: column;
  align-items: center;
  &__date {
    font-size: 12px;
  }
  &__all-button {
    display: flex;
  }
}

.description {
  &__element-preview {
    overflow: hidden;
  }
}

.button-disabled {
  background-color: #ff6767;
}
</style>
