<script setup>
import  {computed, reactive, ref } from "vue";

import Button from "@/components/Button.vue";


let HomeWorks = reactive([
  {
    id : 1 ,
    name : "Math HomeWork",
    category :'University',
    status : false
  },
  {
    id : 2 ,
    name : "Phys HomeWork",
    category :'University',
    status : false
  },
  {
    id : 3 ,
    name : "History HomeWork",
    category :'University',
    status : false
  },

]); // Each Homework is an Object
let CompletedHomeworks = computed( () => {
  return HomeWorks.filter(a => a.status).length
});  // Show Only Completed Homework
let unCompletedHomeworks = computed( () => {
  return HomeWorks.filter(a => !a.status).length
}); // Show unCompleted Homework
let Category = ['University','Work','Personal']
let HomeWorkAdd = ref('');
let selected = ref('Category');
let selectedtoFilter = ref('All');

function ResetInput() {
  HomeWorkAdd.value = '';
  selected.value = 'Category'
}
function DeleteAllTask() {
  for (let i = 0; i < HomeWorks.length; i++)
  {
    delete HomeWorks[i]
  }
}
function AddTask(HomeWorkAdd,selected ){
  if(HomeWorkAdd.length === 0 || selected === 'Category' ) {
    alert("You Have Missed to Insert a Name or Category")
  }
  else {
    HomeWorks.push({
      id : HomeWorks.length+1 ,
      name : HomeWorkAdd ,
      category :selected,
      status : false
    })
    ResetInput()
  }
}
</script>
<template>
<div class="bg-white p-6  mt-4 mb-4 rounded-lg">
<h2 class="font-bold mb-4 text-center"> To Do List</h2>
  <div  class="grid grid-cols-3 text-left font-bold mb-3">
    <div >Name</div>
    <div class="text-center">
      <select v-model="selectedtoFilter"  class="rounded" >
        <option> All</option>
        <option v-for="item in Category">{{item}}</option>
      </select>
    </div>
    <div>Status</div>
  </div>

  <div class="divide-y">

    <ul v-show="unCompletedHomeworks" v-for="item in HomeWorks.filter(item => item.status || item.category === selectedtoFilter)" :key="item.id" class="grid grid-cols-3">
      <li class="flex justify-between items-center mr-4 ">{{item.name}}</li>
      <p class="text-center"> {{item.category}}</p>
      <div class="flex justify-between">
        <input type="checkbox" v-model="item.status" class="ml-2">
        <button class="font-bold text-base text-red-800"  @click="delete HomeWorks[item.id-1]">Delete</button>
      </div>
    </ul>
  </div>





  <div class="border-b-4 border-blue-700 mt-4 mb-4"></div>
  <div class="divide-y">
    <ul v-show="CompletedHomeworks" v-for="item in HomeWorks.filter( item => item.status && item.category === selectedtoFilter )" :key="item.id" class="grid grid-cols-3">
      <li class="flex justify-between items-center mr-4 ">{{item.name}}</li>
      <p class="text-center"> {{item.category}}</p>
      <div class="flex justify-between">
        <input type="checkbox" v-model="item.status" class="ml-2">
        <button class="font-bold text-base text-red-800"  @click="delete HomeWorks[item.id-1]">Delete</button>
      </div>
    </ul>
  </div>


</div>

<form @submit.prevent="AddTask(HomeWorkAdd ,selected )"  class="flex flex-col mt-4">
    <input type="text" placeholder="Add Task..." class="border mb-3" v-model="HomeWorkAdd">
  <div class="flex">
    <Button title="Add Task" class="w-full mr-1 "/>
    <select v-model="selected" class="rounded p-2 px-5" >
      <option disabled v-text="selected"></option>
      <option v-for="item in Category">{{item}}</option>
    </select>
  </div>
  </form>
  <div class="mt-3">
    <Button title="Delete All Tasks" class="bg-red-600 hover:bg-red-800 w-full"  @click="DeleteAllTask"/>
  </div>

</template>

