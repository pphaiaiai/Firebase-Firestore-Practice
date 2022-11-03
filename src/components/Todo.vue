<script>
import { query, collection, getDocs } from "firebase/firestore"
import db from "../firebase/init.js"

export default {
    name: "Test",
    created(){
        this.getTodos();
    },
    data(){
        return {
            todos: []
        }
    },
    props: {},
    methods: {
        async getTodos() {
            const querySnap = await getDocs(query(collection(db, "todos")));

            querySnap.forEach((doc) => {
                this.todos.push(doc.data());
            });
        }
    }
};
</script>
 
<template>
    <ul>
        <li v-for="todo in todos" :key="todo.title">
            {{ todo.title }}
        </li>
    </ul>
</template>
 
<style lang="scss" scoped>
</style>