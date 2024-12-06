<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const key = '13e27bcd'
const inputValue = ref('')
const submitedValue = ref('')
const movies = ref([])

const handleSubmit = () => {
    submitedValue.value = inputValue.value
    data(submitedValue.value)
    submitedValue.value = ''
  }

  const data = async(search) => {
    try {
      
      const res =  await axios.get(`http://www.omdbapi.com/`, {
        params: {apiKey: key, s: search}
      })
      console.log(res.data)
      movies.value = res.data.Search


    } catch (error) {
      console.log(error)
    }
  }
</script>

<template>

  <div class="bg-gray-100 w-full">
    <div class="bg-black p-3 xs:text-center flex xs:flex-col md:flex-row md:justify-between xs:w-full">

      <h1 class="text-xl text-slate-50 content-center">THE MOVIES API</h1>

      <form @submit.prevent="handleSubmit" class="flex">
        <input type="text" placeholder="search by title" v-model="inputValue" 
        class="text-slate-800 border-2 focus:outline-none w-full">
        <button type="Submit" class="text-gray-700 bg-green-300 p-2 rounded-r-md">Search</button>
      </form>
    </div>

    <p class="p-2 text-center"
      v-if="!movies.length">
      Start searching!</p>

    <ul v-if="movies.length" class="md:grid md:grid-cols-2 xl:grid-cols-3 xs:flex xs:flex-col">
      <li v-for="x in movies" :key="x.imdbID" class="bg-slate-200 m-3 p-2 w-fit">
        <p class="w-80">{{x.Title}}</p>
        <img :src="x.Poster" alt="" >
        <p v-if="x.Poster == 'N/A'">Image not available</p>
        <p>Year: {{x.Year}}</p>
      </li>
    </ul>
  </div>

</template>

<style scoped>

</style>