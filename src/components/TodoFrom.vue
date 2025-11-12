<template>
  <form
    @submit.prevent="handleSubmit"
    class="flex gap-3 bg-white backdrop-blur-sm rounded-2xl p-6 shadow-sm"
  >
    <input
      v-model="newTodo"
      type="text"
      placeholder="Add new activity!"
      class="flex-1 py-3.5px-5 border-2 border-gray-200 rounded-xl focus:outline-none focus:border-purple-500 transition-all duration-300"
      maxlength="100"
    />
    <button
      type="submit"
      class="flex items-center gap-2 py-3.5 px-6 bg-linear-to-br from-purple-500 to-purple-700 text-white rounded-xl font-semibold shadow-md hover:shadow-lg disabled:opacity-50 transition-all duration-300"
      :disabled="!newTodo.trim()"
    >
      <span class="text-xl font-bold">+</span>
      Add Todo
    </button>
  </form>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const newTodo = ref('')
const emit = defineEmits<{
  (e: 'addTodo', title: string): void
}>()

const handleSubmit = () => {
  emit('addTodo', newTodo.value)
  newTodo.value = ''
}
</script>
