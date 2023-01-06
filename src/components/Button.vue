<script setup>
let props = defineProps({
  title : String
})

</script>


<template>

  <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded ">
   {{props.title}}
  </button>

</template>