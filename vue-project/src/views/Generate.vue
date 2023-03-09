<template>
  <section v-if="step === 1" class="max-w-2xl mx-auto">
    <div class="pt-20 pb-10">
      <h1 class="text-2xl text-orange-400 text-center">Please select your image</h1>
    </div>
    <div>
      <div class="bg-slate-100 grid grid-cols-3 gap-5 py-2.5 px-2 rounded">
        <img v-for="(item, key) in data" :key="key" @click="active(item)" :class="item.id === storedImg?.id ? 'border-cyan-300' : 'border-red-300'" :src="'/asset/' + item.name" alt="" class="mx-auto border-2 active:border-cyan-300 hover:border-cyan-300 hover:cursor-pointer">
      </div>      
    </div>
    <div class="flex justify-center">
      <button @click="selectImage" class="py-2.5 px-4 bg-cyan-700 text-white rounded mt-5">
        Next
      </button>
    </div>
  </section>
  <section class="grid grid-cols-12 gap-10 max-w-6xl mx-auto pt-10" v-else-if="step === 2">
    <div class="col-span-6">
      <div class=" border-2 p-2 border-rose-600 mb-10 rounded max-h-64">
        <p class="">
          Input
        </p>
        <div class="max-h-32 overflow-y-auto">
          <div v-for="(item, key) in dataInput" :key="key" class="flex gap-4 pb-5">
            <input v-model="item.text" type="text" class="border border-slate-900">
            <button @click="remove(key)" class="py-2 px-5 bg-red-400 rounded">
              delete
            </button>
          </div>
        </div>
        <div class="">
          <button @click="add" class="py-2 px-5 bg-cyan-400 rounded">
            add
          </button>
        </div>
      </div>
      <div class="bg-slate-100 p-5 border-2 border-cyan-500 rounded max-h-64 overflow-y-auto">
        <p>
          Config
        </p>
        <div>
          <div class="flex gap-1 pt-10 pb-5">
            <label for="gravity">Gravity</label>
              <select v-model="position.gravity" name="gravity" id="gravity">
                <option value="NorthWest">NorthWest</option>
                <option value="North">North</option>
                <option value="NorthEast">NorthEast</option>
                <option value="West">West</option>
                <option value="Center">Center</option>
                <option value="East">East</option>
                <option value="SouthWest">SouthWest</option>
                <option value="South">South</option>
                <option value="SouthEast">SouthEast</option>
              </select>
          </div>
          <div class="flex gap-4">
            <div>
              <p>X: </p>
              <input v-model="position.x" type="number">
            </div>
            <div>
              <p>Y: </p>
              <input v-model="position.y" type="number">
            </div>
          </div>
          <div class="flex pt-5 gap-1">
            <label for="color">Font color: </label>
            <input @change="getColor($event)" type="color" id="color" name="color" :value="position.fontColor" class="">
          </div>
          <div class="flex pt-5 gap-1">
            <label for="color">Stroke color: </label>
            <input @change="getStroke($event)" type="color" id="color" name="color" :value="position.strokeColor" class="">
          </div>
          <div>
            <label for=""></label>
          </div>
        </div>
      </div>
    </div>
    <div class="col-span-6">
      <p class="text-2xl text-orange-400 text-center">
        Hi
      </p>
      <img :src="'/asset/' + storedImg.name" alt="">
      <div class="flex justify-center mt-10">
        <a :href="createSource(dataInput[0].text)" target="_blank" class="py-2.5 px-4 bg-cyan-700 text-white rounded">Preview</a>
      </div>
    </div>
    <div class="col-span-12 flex justify-center py-10">
      <button @click="step = 3" class="py-2.5 px-4 bg-cyan-700 text-white rounded">
        Generate
      </button>
    </div>
  </section>
  <section v-else-if="step === 3" class="max-w-4xl mx-auto mt-10 bg-slate-200">
    <div class="grid grid-cols-12 gap-5 p-4">
      <img v-for="(item, key) in dataInput" :key="key" :src="createSource(item.text)" alt="" class="col-span-4 border-red-400 border-2">
    </div>
  </section>
</template>

<script setup>
  import {ref} from 'vue'
  import axios from 'axios'
  // import img from '../assets/7wsh.png'
  import config from '../../public/asset/config.json'

  
  const url = 'https://267d-114-125-94-123.ap.ngrok.io'

  // step 1

  const step = ref(1)
  const storedImg = ref(null)
  const position = ref({
    x: 0,
    y: 0,
    gravity: "Center",
    text: "",
    font: "Helvetica.ttf",
    fontSize: 32,
    fontColor: "#000",
    strokeColor: "#000",
    strokeWidth: 0
  })

  const active = (val) => {
    storedImg.value = val
  }

  const selectImage = () => {
    position.value.x = storedImg.value.x
    position.value.y = storedImg.value.y
    position.value.gravity = storedImg.value.gravity
    position.value.font = storedImg.value.font
    position.value.fontSize = storedImg.value.fontSize
    position.value.fontColor = storedImg.value.fontColor
    position.value.strokeColor = storedImg.value.strokeColor
    step.value = 2
  }

  axios.get(`${url}/v1/template`,{
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Headers': '*'
    }
  })
  .then((response) => {
    return console.log(response)
  })

  const data = ref(config)



  // step 2

  const add = () => {
    dataInput.value.push({name: ''})
  }

  const remove = (i) => {
    dataInput.value.splice(i, 1)
  }

  const getColor = (event) => {
    position.value.fontColor = event.target.value
  }

  const getStroke = (event) => {
    position.value.strokeColor = event.target.value
  }

  const dataInput = ref([
    {text: ''}
  ])


  // step 3

  const createSource = (text) => {
    const uri = encodeURI(`${url}/v1/create-or-get/${storedImg.value.id}?gravity=${position.value.gravity}&font=${position.value.font}&size=${position.value.fontSize}&fill=${encodeURIComponent(position.value.fontColor.replace('#', ''))}&stroke=${position.value.strokeColor.replace('#', '')}&text=${text}&x=${position.value.x}&y=${position.value.y}`)
    return uri
  }

</script>