<template>
  <section>
    <div class="">
      <h1>Please select your image</h1>
    </div>
    <div>
      <div class="bg-slate-100 grid grid-cols-3 gap-5 py-2.5">
        <img @click="active(data, item)" :class="item.active ? 'border-cyan-300' : 'border-red-300'" v-for="(item, key) in data" :key="key" :src="item.image" alt="" class="w-5/12 mx-auto border-2 active:border-cyan-300 hover:border-cyan-300 hover:cursor-pointer">
      </div>      
    </div>
    <RouterLink to="/generate">Next</RouterLink>
  </section>
</template>

<script setup>
  import { ref } from 'vue'
  import img from '../assets/7wsh.png'


  const active = (val, i) => {
    for(let j = 0; j < val.length; j++) {
      if(val[j].active) val[j].active = false
    }
    i.active = !i.active
  }

  const data = ref([
    {
      id: '1',
      image: img,
      active: false
    },
    {
      id: '2',
      image: img,
      active: false
    },
    {
      id: '3',
      image: img,
      active: false
    },
    {
      id: '4',
      image: img,
      active: false
    },
    {
      id: '5',
      image: img,
      active: false
    },
    {
      id: '6',
      image: img,
      active: false
    },
  ])

</script>