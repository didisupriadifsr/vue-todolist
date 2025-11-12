<template>
  <div class="max-w-2xl mx-auto px-4 pb-12 items-center justify-center">
    <TodoForm @addTodo="addNewTodo" />

    <ul class="bg-white/90 backdrop-blur-sm p-6 space-y-3">
      <li v-for="(item, key) in sortedList" :key="key">
        <TaskItem :isChecked="item.checked" @change="updateItem(item)">
          {{ item.title }}
        </TaskItem>
      </li>
    </ul>
  </div>
</template>
<script setup lang="ts">
import TaskItem from './TaskItem.vue'
import { computed, ref, type Ref, onMounted } from 'vue'
import TodoForm from './TodoFrom.vue'

type Item = {
  title: string
  checked?: boolean
}

const storageItems: Ref<Item[]> = ref([])

const getFromStorage = (): Item[] | [] => {
  const stored = localStorage.getItem('todo-list')
  if (stored) {
    return JSON.parse(stored)
  }
  return []
}

const setToStorage = (items: Item[]): void => {
  localStorage.setItem('todo-list', JSON.stringify(items))
}

const initListItems = (): void => {
  if (storageItems.value?.length === 0) {
    const listItems: Item[] = [
      { title: 'Install the Vue.js', checked: true },
      { title: 'Make the todolist app', checked: false },
      { title: 'Add a todo function', checked: false },
      { title: 'Create a edit todo function', checked: false },
      { title: 'Delete a todo', checked: false },
      { title: 'Push to github repository', checked: false },
      { title: 'Publish my work', checked: false },
    ]
    setToStorage(listItems)
    storageItems.value = listItems
  }
}

const updateItem = (item: Item): void => {
  const updatedItem = findItemInList(item)
  if (updatedItem) {
    toggleItemChecked(updatedItem)
    setToStorage(storageItems.value)
  }
}
// membandingkan title
const findItemInList = (item: Item): Item | undefined => {
  return storageItems.value.find((itemInList: Item) => itemInList.title === item.title)
}
// merubah status checklist
const toggleItemChecked = (item: Item): void => {
  item.checked = !item.checked
}

// sort unchecked di paling atas
const sortedList = computed(() => {
  return [...storageItems.value].sort((a, b) => (a.checked ? 1 : 0) - (b.checked ? 1 : 0))
})

const addNewTodo = (title: string): void => {
  const newItem: Item = {
    title,
    checked: false,
  }
  storageItems.value.unshift(newItem)
  setToStorage(storageItems.value)
}

onMounted(() => {
  storageItems.value = getFromStorage()
  initListItems()
})
</script>
