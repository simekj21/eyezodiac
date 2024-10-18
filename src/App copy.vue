<script setup>
import HelloWorld from './components/HelloWorld.vue'
import { ref, onMounted } from 'vue'
import { zodiacSigns } from './zodiac-signs';

onMounted(() => {
  console.log('mounted', zodiacSigns);
})

</script>

<template>
  <div>
    <h1 class="text-3xl text-red-600 font-bold underline">
      EYE - ZODIAC
    </h1>

    <!-- <img v-for="sign in zodiacSigns" :key="sign.name" :src="sign.imageUrl" :alt="sign.imageAlt" class="logo" /> -->

    <div v-for="sign in zodiacSigns" :key="sign.name" class="flex flex-col items-center">
      <img :src="sign.imageUrl" :alt="sign.imageAlt" class="logo" />
      <h2 class="text-xl text-blue-600 font-bold">{{ sign.name }}</h2>

      <div v-for="trait in sign.klady" :key="trait" class="text-lg text-gray-600 flex flex-row gap-4">
        <div>{{ trait }}</div>
        ANO<input type="checkbox" class="form-checkbox h-5 w-5 text-blue-600" checked />
      </div>
      <div v-for="trait in sign.slabiny" :key="trait" class="text-lg text-red-600 flex flex-row gap-4">
        {{ trait }}
        ANO<input type="checkbox" class="form-checkbox h-5 w-5 text-red-600" />
      </div>

    </div>

  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
