<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue';

interface Todo {
  id: number
  text: string
  isDone: boolean
}

const newTodoText = ref("")
const todos = ref(<Todo[]>([]))
const unfinishTodo = computed(() => {
  return todos.value.filter((t) => !t.isDone)
})

type FilterType = 'All' | 'Not Done' | 'Done';

// 2. 创建响应式变量，默认值为 'All'
const filterStatus = ref<FilterType>('Not Done');

const filterTodos = computed(() => {
  switch (filterStatus.value) {
    case "Not Done":
      return todos.value.filter((t) => !t.isDone)
    case "Done":
      return todos.value.filter((t) => t.isDone)
    default:
      return todos.value
  }
})

const handleAddTodo = () => {
  if (!newTodoText.value) return
  todos.value.unshift({
    id: todos.value.length,
    text: newTodoText.value,
    isDone: false
  })
  newTodoText.value = ""
}

const handleDeleteTodo = (id: number) => {
  todos.value = todos.value.filter(t => t.id !== id)
}

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("pdnode-todos") ?? "[]")
})

watch(todos, (newVal) => {
  localStorage.setItem("pdnode-todos", JSON.stringify(newVal))
}, { deep: true })

</script>

<template>
  <section class="flex justify-center items-start min-h-screen pt-12">
    <div class="card w-100 max-h-[70vh] flex flex-col overflow-hidden bg-base-300 justify-center items-center gap-2">
      <h3 class="text-center font-bold text-2xl card-title text-base-content pt-10">Pdnode Todos</h3>
      <div class="card-body">
        <div class="flex justify-start items-center flex-col gap-4 min-h-0">
          <div class="flex gap-2 w-full">
            <input @keyup.enter="handleAddTodo" v-model="newTodoText" type="text" placeholder="Input New Todo Text"
              class="flex-1 min-w-0 input text-base-content" />
            <button v-on:click="handleAddTodo" class="btn">Add Todo</button>
          </div>

          <div class="tabs tabs-boxed justify-center bg-transparent mb-2">
            <a v-for="status in ['All', 'Not Done', 'Done']" :key="status"
              :class="['tab tab-sm', filterStatus === status ? 'tab-active' : '']"
              @click="filterStatus = status as FilterType">
              {{ status }}
            </a>
          </div>

          <p v-if="unfinishTodo.length === 0 && filterStatus === 'Not Done'">No unfinished tasks</p>

          <ul class="w-full flex-1 list overflow-y-auto max-h-[60vh] relative">
            <TransitionGroup name="list">
              <li v-for="todo in filterTodos" v-bind:key="todo.id" class="list-row">
                <div class="flex justify-between items-center w-full gap-2">
                  <div class="gap-2 flex justify-center items-center">
                    <input v-model="todo.isDone" :binary="true" class="input checkbox checkbox-sm" type="checkbox"
                      @click.stop />

                    <span :class="{ 'line-through opacity-50 text-base-content/50': todo.isDone }">
                      <!-- {{ slotProps.option.text }} -->
                      <input v-bind:value="todo.text" type="text" placeholder="Update your Todo"
                        class="input flex-1 input-ghost text-base-content text-base" />
                    </span>
                  </div>


                  <button v-on:click="handleDeleteTodo(todo.id)" class="btn btn-error btn-soft"><svg
                      xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
                      <path fill="currentColor"
                        d="M6 19a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V7H6zM8 9h8v10H8zm7.5-5l-1-1h-5l-1 1H5v2h14V4z" />
                    </svg></button>

                </div>
              </li>
            </TransitionGroup>
          </ul>

        </div>

      </div>
    </div>
  </section>
</template>

<style scoped>
/* 1. 进入过程中的初始状态：从透明、缩小开始 */
.list-enter-from {
  opacity: 0;
  transform: scale(0.9);
}

/* 2. 进入和离开的过程动画 */
.list-enter-active,
.list-leave-active {
  transition: all 0.4s ease;
}

/* 3. 离开时的处理：必须 absolute 才能让位给新进入的元素 */
.list-leave-active {
  position: absolute;
  width: 100%;
  z-index: 0;
}

/* 4. 离开后的终点状态 */
.list-leave-to {
  opacity: 0;
  transform: scale(0.9);
}

/* 5. 排序移动动画 */
.list-move {
  transition: all 0.4s ease;
}
</style>