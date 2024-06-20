<script setup>
import { onMounted, reactive, ref } from 'vue'

let posts = reactive(ref())

onMounted(() => {
  PostDataService.getAll()
  .then(response => response.json())
  .then(response => {
    console.log(response.data)
  })
})
</script>

<template>
</template>


