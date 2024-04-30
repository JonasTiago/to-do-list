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
      noEdit:boolean;
  }

  const tasks = ref<Task[]>([
    {
    id: 1,
    task: "Estudar Vue.js",
    done: false,
    noEdit:true,
  },
  {
    id: 2,
    task: "Listar tarefas",
    done: false,
    noEdit:true,
  },
  {
    id: 3,
    task: "Adicionar tarefas",
    done: false,
    noEdit:true,
  },
  {
    id: 4,
    task: "Excluir tarefa",
    done: false,
    noEdit:true,
  }
  ]);

  const newTask = ref<string>("");
  const editTaskValue = ref<string>("")
  const countTask = ref<number>(tasks.value.length)
  const countTaskOk = ref<number>(tasks.value.filter(task => task.done === false).length)

  function addTask(){
    if(newTask.value.length > 0){
      tasks.value.push({
        id: tasks.value.length+1,
        task: newTask.value,
        done: false,
        noEdit:true,
      })
      countTask.value++
      
      
      newTask.value = "";
      
    }
  }

  function deleteTask(index:number){
    tasks.value.splice(index,1);
    countTask.value--
    countTaskOk.value = tasks.value.filter(task => task.done === false).length
  }

  function toggleDone(index: number) {
    const task = tasks.value[index];
    task.done = !task.done;
    task.noEdit = true;
    countTaskOk.value = tasks.value.filter(task => task.done === false).length
  }

  function editTask(index: number){
    
    const task = tasks.value[index];

    if(!task.done) {

      if(!task.noEdit && editTaskValue.value.length>0){
          task.task = editTaskValue.value;
          editTaskValue.value = ""
        }else{
          const editTaskOn = tasks.value.filter(tasks => tasks.noEdit == false)[0]
          if(editTaskOn) editTaskOn.noEdit = true;
        }

        editTaskValue.value = task.task;

        task.noEdit = !task.noEdit;
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
      <div class="todo-list">
        <div v-for="(task, index) in tasks" >
        <ToDoItem v-bind="task" :key="task.id" :toggleDone="toggleDone" :index="index">
          <ButtonToDo icon="pi pi-pen-to-square" color="#4147D5"  @click="editTask(index)"/>
          <ButtonToDo icon="pi pi-trash" color="#D62828" @click="deleteTask(index)"/>
        </ToDoItem> 
        <div class="edition" :class="{  none:task.noEdit }">
          <InputForm v-model:value="editTaskValue" />
          <ButtonForm name="Edit" @click="editTask(index)"/>
        </div>
      </div> 
      </div>
      
      <p v-if="countTask" class="countTask">Você tem {{ countTaskOk }} - {{ countTask }} 
        {{ countTask > 1 ? "tarefas" : "tarefa"}} para fazer </p>
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
