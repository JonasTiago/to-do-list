<script setup lang="ts">
  import { ref } from "vue"

  import ButtonForm from "./components/form/ButtonForm.vue"
  import InputForm from "./components/form/InputForm.vue"
  import ToDoItem from "./components/ToDoItem.vue";
  import ButtonToDo from "./components/ButtonToDo.vue";

  interface Task {
      id: number;
      task: string;
      done: boolean;
      edit:boolean;
      date: Date;
  }

  const tasks = ref<Task[]>([
    {
    id: 1,
    task: "Estudar Vue.js",
    done: false,
    edit:true,
    date: new Date()
  },
  {
    id: 2,
    task: "Trabalhar",
    done: true,
    edit:true,
    date: new Date()
  },
  {
    id: 3,
    task: "Dormir",
    done: false,
    edit:true,
    date: new Date()
  },
  {
    id: 4,
    task: "Comer",
    done: false,
    edit:true,
    date: new Date()
  },{
    id: 4,
    task: "Comer",
    done: false,
    edit:true,
    date: new Date()
  },
  ]);

  const newTask = ref<string>("");
  const editTaskValue = ref<string>("")
  const countTask = ref<number>(tasks.value.length)

  function addTask(){
    console.log(newTask.value)
    if(newTask.value.length > 0){
      tasks.value.push({
        id: tasks.value.length+1,
        task: newTask.value,
        done: false,
        edit:true,
        date: new Date()
    })
    countTask.value++

    newTask.value = "";

    }
  }

  function deleteTask(index:number){
    tasks.value.splice(index,1);
    countTask.value--
  }

  function toggleDone(index: number) {
    const task = tasks.value[index];
    task.done = !task.done;
  }

  function editTask(index: number){
    
    const task = tasks.value[index];

    if(!task.done) {
        if(!task.edit && editTaskValue.value.length>0){
          task.task = editTaskValue.value;
          editTaskValue.value = ""
        }

        editTaskValue.value = task.task;

      task.edit = !task.edit;
    }

   
    
  }
  
</script>

<template>
  <div class="container">
      <h1>To-Do List</h1>
      <div class="form">
        <InputForm v-model:value="newTask" placeholder="O que você quer fazer?"/>
        <ButtonForm name="Create" @click="addTask"/>
      </div> 
      <div v-for="(task, index) in tasks" class="todolist">
        <ToDoItem v-bind="task" :key="task.id" :toggleDone="toggleDone" :index="index">
          <ButtonToDo icon="pi pi-pen-to-square" color="#4147D5"  @click="editTask(index)"/>
          <ButtonToDo icon="pi pi-trash" color="#D62828" @click="deleteTask(index)"/>
        </ToDoItem> 
        <div class="edition" :class="{  none:task.edit }">
          <InputForm v-model:value="editTaskValue" />
          <ButtonForm name="Edit" @click="editTask(index)"/>
        </div>
      </div> 
      <p class="count">You have {{ countTask }} task{{ countTask > 1 ? "s" : ""}} to do</p>
  </div>
  
</template>

<style scoped>

  .container{
    display: flex;
    align-items: center;
    flex-direction: column;
    margin: 4rem auto;
  }

  h1{
      font-family: "Ubuntu", sans-serif;
      font-weight: bold;
      font-style: normal;
      font-size: 3rem;
      margin-bottom: 1.5rem;
  }

  .todolist{
    width: 40%;
  }
  
  .form{ 
    display: flex;
    justify-content: space-around;
    width: 40%;
    margin-bottom: 2rem;
  }

  .edition{
    display: flex;
    margin: 0.5rem 0;

  }

  .none{
    display: none;
  }

  .count{
    font-family: "Ubuntu", sans-serif;
      font-weight: bold;
      font-style: normal;
      font-size: 2rem;
      margin: 1.5rem;
      position: fixed;
      bottom: 0;
  }

</style>
