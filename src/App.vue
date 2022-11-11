<script setup>
import {ref, onMounted} from 'vue'
import {query, collection, getDocs, doc, updateDoc, deleteDoc} from 'firebase/firestore'
import db from './firebase/init.js'
import TodoList from './components/TodoList.vue'
import TodoInput from './components/TodoInput.vue'

const todos = ref([])

async function getTodos() {
  const querySnap = await getDocs(query(collection(db, 'todos')))
  todos.value = []
  querySnap.forEach((doc) => {
    let data = doc.data()
    data.id = doc.id
    todos.value.push(data)
  })
}

async function toggleCompleted(index) {
  const todo = doc(db, "todos", todos.value[index].id)
  await updateDoc(todo, {
    completed: !todos.value[index].completed,
  })
  getTodos()
}

async function deleteTodo(index) {
  const todo = doc(db, "todos", todos.value[index].id)
  await deleteDoc(todo)
  getTodos()
}

onMounted(() => {
  getTodos()
})
</script>
 
<template>
  <h1>Todo List</h1>
  <TodoInput @update-todos="getTodos" />
  <TodoList :todos="todos" @toggle-completed="toggleCompleted" @delete-todo="deleteTodo" />
</template>
 
<style>

</style>

