<template>
    <h4>{{ add }}</h4>
    <input type="text" v-model="add" />
    <div style="text-align: center">
        <h3>Count value: {{ count }}</h3>
        <button @click="incrementCount">Increment count</button>

        <h2>Todo List</h2>
        <div
            style="
                display: grid;
                grid-template-columns: 70% 15%;
                justify-content: space-evenly;
                margin: 1rem 0;
            "
        >
            <input
                type="text"
                name="todo"
                v-model="newTodo"
                @keydown.enter="addTodo"
            />
            <button @click="addTodo">Add new todo</button>
        </div>
        <div
            v-for="(todo, index) in todos"
            :key="index"
            style="
                display: grid;
                grid-template-columns: 20% 20% 20%;
                justify-content: space-evenly;
            "
        >
            <span style="text-align: left; padding-left: 5rem">{{ todo }}</span>
            <button @click="deleteTodo(index)">Delete</button>
            <button @click="editTodo(todo, index)">Edit</button>
        </div>
        <dialog :open="editDialog">
            <input
                type="text"
                v-model="editedTodo"
                @keydown.enter="saveEdit(toEditTodo)"
                autofocus
            />
            <button @click="saveEdit(toEditTodo)">Save</button>
        </dialog>
    </div>
</template>

<script setup>
import { ref } from "vue";
const add = ref("test");
const count = ref(0);
const todos = ref([]);
const newTodo = ref(null);
const editDialog = ref(false);
const editedTodo = ref(null);
const toEditTodo = ref(null);

function incrementCount() {
    count.value++;
}

function addTodo() {
    if (!newTodo.value) return;
    todos.value.push(newTodo.value);
    newTodo.value = null;
}

function deleteTodo(index) {
    todos.value.splice(index, 1);
}

function editTodo(todo, index) {
    editDialog.value = true;
    editedTodo.value = todo;
    toEditTodo.value = index;
}

function saveEdit(index) {
    todos.value[index] = editedTodo.value;
    editDialog.value = false;
}
</script>
