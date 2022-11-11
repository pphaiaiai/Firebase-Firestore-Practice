<script setup>
import {collection, addDoc, serverTimestamp} from '@firebase/firestore';
import {ref} from 'vue'
import db from '../firebase/init.js'

const emit = defineEmits(['update-todos'])
const newTitle = ref('')

async function addTodo(){
    await addDoc(collection(db, 'todos'), {
        title: newTitle.value,
        completed: false,
        createdAt: serverTimestamp(),
    })
    newTitle.value = ''
    emit('update-todos')
}
</script>
 
<template>
    <div>
        <input v-model="newTitle" type="text" placeholder="Enter a new task" size="30">
        <button @click="addTodo">Add</button>
    </div>
</template>
 
<style>

</style>