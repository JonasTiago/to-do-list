<script setup lang="ts">
  import { ref, computed } from "vue"

  import ButtonForm from "./components/form/ButtonForm.vue"
  import InputForm from "./components/form/InputForm.vue"
  import ToDoItem from "./components/ToDoItem.vue";
  import ButtonToDo from "./components/ButtonToDo.vue";

  interface Task {
      id: number;
      task: string;
      done: boolean;
      selectEdit:boolean;
  }

  const tasks = ref<Task[]>([
    {
    id: 1,
    task: "Estudar Vue.js",
    done: false,
    selectEdit:false,
  },
  {
    id: 2,
    task: "Listar tarefas",
    done: false,
    selectEdit:false,
  },
  {
    id: 3,
    task: "Adicionar tarefas",
    done: false,
    selectEdit:false,
  },
  {
    id: 4,
    task: "Excluir tarefa",
    done: false,
    selectEdit:false,
  }
  ]);

  const inputNewTask = ref<string>("");
  const inputTaskEdit = ref<string>("")

  function addTask(){

    if(inputNewTask.value.length > 0){
      tasks.value.push({
        id: tasks.value.length+1,
        task: inputNewTask.value,
        done: false,
        selectEdit:false,
      })
      inputNewTask.value = "";  
    }
      
  }
  
  function deleteTask(index:number){
    tasks.value.splice(index, 1);
  }

  function toggleDone(index: number) {
    const task = tasks.value[index];
    task.done = !task.done;
    task.selectEdit = false;
  }

  function editTask(index: number){
    
    const task = tasks.value[index];

    if(!task.done) {

      if(task.selectEdit){
          task.task = inputTaskEdit.value;
          inputTaskEdit.value = ""
      } 
      else {
          const editTaskOn = tasks.value.filter(tasks => tasks.selectEdit)[0]
          if(editTaskOn) editTaskOn.selectEdit = false;
          inputTaskEdit.value = task.task;
      }


      task.selectEdit = !task.selectEdit;
    }

  }

  const countTask = computed(() => {
    return {
      total: tasks.value.length,
      done: tasks.value.filter(task => !task.done).length,
    }
  })

  defineEmits<{
    change: [index: number] 
  }>()
  
</script>

<template>
  <div class="container">
      <h1>To-Do List</h1>

      <div class="form">
        <InputForm v-model:value="inputNewTask" placeholder="O que você quer fazer?"/>
        <ButtonForm name="Create" @click="addTask"/>
      </div> 
      
      <div class="todo-list">
        <div v-for="(task, index) in tasks" >
        <ToDoItem 
          v-bind="task" 
          :key="task.id"
          @change="toggleDone(index)"
        >
          <ButtonToDo 
            icon="pi pi-pen-to-square" 
            color="#4147D5"  
            @click="editTask(index)"
            />
          <ButtonToDo icon="pi pi-trash" color="#D62828" @click="deleteTask(index)"/>
        </ToDoItem> 
        <div class="edition" :class="{  none:!task.selectEdit }">
          <InputForm v-model:value="inputTaskEdit"/>
          <ButtonForm name="Edit" @click="editTask(index)"/>
        </div>
      </div> 
      </div>
      
      <p 
        v-if="countTask.total > 0" 
        class="countTask">
        Você tem {{ countTask.done }} - {{ countTask.total }} 
        {{ countTask.total > 1 ? "tarefas" : "tarefa"}} para fazer 
      </p>
      <p v-else class="noTask">Crie novas tarefas!!!</p>
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
      position: fixed;
      top: 1.5rem;
  }

  .todo-list{
    width: 40%;
    max-height: 65vh;
    overflow-y: scroll;
    position: fixed;
    top: 10rem;
  }

  .todo-list::-webkit-scrollbar {
    display: none;
  }
  
  .form{ 
    display: flex;
    justify-content: space-around;
    width: 40%;
    position: fixed;
    top: 6rem;
  }

  .edition{
    display: flex;
    margin: 0.5rem 0;

  }

  .none{
    display: none;
  }

  .countTask{
    font-family: "Ubuntu", sans-serif;
      font-weight: bold;
      font-style: normal;
      font-size: 2rem;
      margin: 1.5rem;
      position: fixed;
      bottom: 0;
  }

  .noTask{
    font-family: "Ubuntu", sans-serif;
      font-weight: bold;
      font-style: normal;
      font-size: 2rem;
      margin: 1.5rem;
      position: fixed;
      bottom: 50vh;
  }

</style>
