<script setup>
import  {computed, reactive, ref } from "vue";
import Button from "@/components/Button.vue";


let HomeWorks = reactive([
  {
    id : 1 ,
    name : "Math HomeWork",
    status : false
  },
  {
    id : 2 ,
    name : "Phys HomeWork",
    status : false
  },
  {
    id : 3 ,
    name : "History HomeWork",
    status : false
  },

]); // Each Homework is an Object
let CompletedHomeworks = computed( () => {
  return HomeWorks.filter(a => a.status).length
});  // Show Only Completed Homework
let unCompletedHomeworks = computed( () => {
  return HomeWorks.filter(a => !a.status).length
}); // Show unCompleted Homework
let HomeWorkAdd = ref('');

function ResetInput() {
  HomeWorkAdd.value = '';
}
function DeleteAllTask() {
  for (let i = 0; i < HomeWorks.length; i++)
  {
    delete HomeWorks[i]
  }
}
function AddTask(HomeWorkAdd ){
  if(HomeWorkAdd.length === 0 )
  {
    alert("Insert Something pls")
  }
  else {
    HomeWorks.push({
      id : HomeWorks.length+1 ,
      name : HomeWorkAdd ,
      status : false
    })
    ResetInput()
  }
}

</script>
<template>
<div class="bg-white p-6  mt-4 mb-4 rounded-md">
<h2 class="font-bold mb-4 text-center"> To Do List</h2>


  <div>
    <h2 class="font-bold mb-2 text-left" > inProgress</h2>
    <ul  v-show="unCompletedHomeworks" v-for="item in HomeWorks.filter(item => !item.status)" :key="item.id" class="grid grid-cols-2 ">

      <label >
        <li class="flex justify-between items-center mr-4 font-bold">
           {{item.name}}
          <input type="checkbox" v-model="item.status" class="ml-2">
        </li>
      </label>
      <button class="font-bold text-base text-red-800"  @click="delete HomeWorks[item.id-1]">Delete</button>
    </ul>
  </div>

<div class="border-b-4 border-blue-700 mt-4 mb-4">
</div>


  <div>
    <h2 class="font-bold mb-2 text-left" >Done</h2>
    <ul  v-show="CompletedHomeworks" v-for="item in HomeWorks.filter(item => item.status)" :key="item.id" class="grid grid-cols-2 ">

      <label >
        <li class="flex justify-between items-center mr-4 font-bold">
          {{item.name}}
          <input type="checkbox" v-model="item.status" class="ml-2">
        </li>
      </label>
      <button class="font-bold text-base text-red-800"  @click="delete HomeWorks[item.id-1]">Delete</button>
    </ul>
  </div>





</div>


<form @submit.prevent="AddTask(HomeWorkAdd)"  class="flex flex-col mt-4">
    <input type="text" placeholder="Add Task..." class="border mb-3" v-model="HomeWorkAdd">
    <Button title="Add Task"/>
  </form>
  <div class="mt-3">
    <Button title="Delete All Tasks" class="bg-red-600 hover:bg-red-800 w-full"  @click="DeleteAllTask"/>

  </div>
</template>

