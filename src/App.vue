<script setup lang="ts">
  import { ref } from "vue"

  import ToDoItem from "./components/ToDoItem.vue";

  const newTask = ref<string>("");

  const tasks = ref([
    {
      id: 1,
      task: "Atividade",
      done: true,
      date: new Date()
    }
  ]);

  function addTask(){
    tasks.value.push({
      id: tasks.value.length,
      task: newTask.value,
      done: false,
      date: new Date()
    })

    console.log(tasks)
  }
</script>

<template>
  <div class="container">
      <h1>To-Do List</h1>
      <div class="form">
        <input type="text" v-model="newTask" placeholder="O que você quer fazer?">
        <button type="button" @click="addTask">Create</button>
      </div> 
      <div v-for="(task, index) in tasks">
        <ToDoItem :task="task.task" :key="index"/>
      </div> 
  </div>
  
</template>

<style scoped>

  .container{
    display: flex;
    align-items: center;
    flex-direction: column;
    margin: 5rem auto;
  }

  h1{
      font-family: "Ubuntu", sans-serif;
      font-weight: bold;
      font-style: normal;
      font-size: 4rem;
      margin-bottom: 1.5rem;
    }

  .form{
    display: flex;
    justify-content: space-around;
    width: 30%;
    margin-bottom: 5rem;

    input{
      width: 85%;
      height: 2.5rem;
      margin: 0 1rem;
      padding: 0.5rem;
      border-radius: 5px;
      font-family: "Roboto", sans-serif;
      font-size: 1rem;
    }

    button{
      background-color: #FFC300;
      border-radius: 100px;
      border: none;
      width: 5.2rem;
      height: 2.6rem;
      cursor: pointer;
      color: #381E72;
      font-family: "Roboto", sans-serif;
      font-weight: bold;
      font-style: normal;
      font-size: 1rem;
    }

    button:hover{
      background-color: rgba(255, 196, 0, 0.655);
    }
  }
</style>
