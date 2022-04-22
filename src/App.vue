<template>
  <section class="todoapp">
    <header class="header">
      <h1>todos</h1>
      <input
        class="new-todo"
        autofocus
        autocomplete="off"
        placeholder="What needs to be done?"
        v-model="todoVal"
        @keydown.enter="handleAddClick"
      />
    </header>
    <section class="main">
      <input id="toggle-all" class="toggle-all" type="checkbox" />
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li class="todo" v-for="(item, index) in List" :key="item.id">
          <div class="view">
            <input
              class="toggle"
              type="checkbox"
              v-model="item.isComplete"
              @change="handleToggleClick"
            />
            <label>{{ item.name }}</label>
            <button
              class="destroy"
              @click="handleRemoveClick(item.id, index)"
            ></button>
          </div>
          <input class="edit" type="text" />
        </li>
      </ul>
    </section>
    <footer class="footer">
      <span class="todo-count"> <strong></strong> {{ leftLen }}left </span>
      <ul class="filters">
        <li><a href="#/all" @click="handleAllClick">All</a></li>
        <li><a href="#/active" @click="handleActiveClick">Active</a></li>
        <li>
          <a href="#/completed" @click="handleCompletedClick">Completed</a>
        </li>
      </ul>
      <button class="clear-completed" @click="removeCompleted">
        remaining"> Clear completed
      </button>
    </footer>
  </section>
</template>
<script setup>
import "./assets/index.css";
import { ref, computed } from "vue";
let todoId = ref(1);
let todoVal = ref("");
let leftLen = ref(0);
let todoList = ref([
  {
    id: 1,
    name: "学习Vue",
    isComplete: false,
  },
]);
let identifying = ref("all");
let filters = ref({
  all: function () {
    return todoList.value;
  },
  active: function () {
    return todoList.value.filter((item) => !item.isComplete);
  },
  completed: function () {
    return todoList.value.filter((item) => item.isComplete);
  },
});

const List = computed(() => {
  return filters.value[identifying.value]();
});
// 增加
const handleAddClick = () => {
  if (todoVal.value.trim()) {
    todoList.value.unshift({
      id: ++todoId.value,
      name: todoVal.value,
      isComplete: false,
    });
  }
  leftLen.value = todoList.value.filter((item) => !item.isComplete).length;
  todoVal.value = "";
};
// 删除
const handleRemoveClick = (id, index) => {
  // todoList.value.splice(todoList.value[index],1)
  todoList.value = todoList.value.filter((item) => item.id !== id);
};
// 修改
const handleToggleClick = () => {
  leftLen.value = todoList.value.filter((item) => !item.isComplete).length;
};

// 全选
const handleAllClick = () => {
  identifying.value = "all";
};

// 选中
const handleActiveClick = () => {
  identifying.value = "active";
};

// 完成
const handleCompletedClick = () => {
  identifying.value = "completed";
};
</script>
